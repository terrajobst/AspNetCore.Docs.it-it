---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started
title: 'Utilizzo di Entity Framework 4.0 e controllo ObjectDataSource, parte 1: introduzione | Microsoft Docs'
author: tdykstra
description: Questa serie di esercitazioni si basa sull'applicazione web di Contoso University specificano che viene creato da Getting Started with serie di esercitazioni in Entity Framework. Se yo...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 244278c1-fec8-4255-8a8a-13bde491c4f5
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started
msc.type: authoredcontent
ms.openlocfilehash: ba3b65dededeca3534b9273bfd3ae48429711fce
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369758"
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-1-getting-started"></a>Utilizzo di Entity Framework 4.0 e controllo ObjectDataSource, parte 1: introduzione
====================
da [Tom Dykstra](https://github.com/tdykstra)

> Questa serie di esercitazioni si basa sull'applicazione web Contoso University specificano che viene creato per il [Introduzione a Entity Framework 4.0](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) serie di esercitazioni. Se si non è stato completato le esercitazioni precedenti, come punto di partenza per questa esercitazione è possibile [scaricare l'applicazione](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) verrebbe creato. È anche possibile [scaricare l'applicazione](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) creato dalla serie di esercitazioni complete.
> 
> L'applicazione web di esempio Contoso University illustra come creare applicazioni Web Form ASP.NET usando il Entity Framework 4.0 e Visual Studio 2010. L'applicazione di esempio è un sito Web per una fittizia Contoso University. Include funzionalità, come ad esempio l'ammissione di studenti, la creazione di corsi e le assegnazioni di insegnati.
> 
> L'esercitazione illustra alcuni esempi in c#. Il [esempio scaricabile](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) contiene il codice in c# e Visual Basic.
> 
> ## <a name="database-first"></a>Prima di tutto di database
> 
> Esistono tre modi, è possibile utilizzare i dati in Entity Framework: *Database First*, *Model First*, e *Code First*. Questa esercitazione è destinata prima di Database. Per informazioni sulle differenze tra questi flussi di lavoro e linee guida su come scegliere quello più adatto per lo scenario, vedere [flussi di lavoro sviluppo di Entity Framework](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="web-forms"></a>Web Form
> 
> Ad esempio la serie di guide introduttive, questa serie di esercitazioni viene utilizzato il modello Web Form ASP.NET e si presuppone che si sappia usare con Web Form ASP.NET in Visual Studio. Se non, viene visualizzato [Introduzione a Web Form ASP.NET 4.5](../../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Se si preferisce usare il framework ASP.NET MVC, vedere [Introduzione a Entity Framework con MVC ASP.NET](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> ## <a name="software-versions"></a>Versioni del software
> 
> | **Illustrato nell'esercitazione** | **Si integra inoltre con** |
> | --- | --- |
> | Windows 7 | Windows 8 |
> | Visual Studio 2010 | Visual Studio 2010 Express per Web. L'esercitazione non è stata testata con le versioni successive di Visual Studio. Esistono numerose differenze nelle opzioni del menu, finestre di dialogo e modelli. |
> | .NET 4 | .NET 4.5 è compatibile con .NET 4, ma non l'esercitazione è stata testata con .NET 4.5. |
> | Entity Framework 4 | L'esercitazione non è stata testata con le versioni successive di Entity Framework. A partire da Entity Framework 5, Entity Framework Usa per impostazione predefinita il `DbContext API` che è stata introdotta con Entity Framework 4.1. Il controllo EntityDataSource è stato progettato per utilizzare il `ObjectContext` API. Per informazioni su come usare EntityDataSource controllare con il `DbContext` API, vedere [questo post di blog](https://blogs.msdn.com/b/webdev/archive/2012/09/13/how-to-use-the-entitydatasource-control-with-entity-framework-code-first.aspx). |
> 
> ## <a name="questions"></a>Domande
> 
> Se hai domande che non sono direttamente correlate con l'esercitazione, è possibile pubblicarli per i [forum di ASP.NET Entity Framework](https://forums.asp.net/1227.aspx), il [Entity Framework e LINQ al forum su entità](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/), o [ StackOverflow.com](http://stackoverflow.com/).


Il `EntityDataSource` controllo consente di creare un'applicazione molto rapidamente, ma è possibile mantenere una quantità significativa di logica di business e logica di accesso ai dati richiede in genere il *aspx* pagine. Se si prevede che l'applicazione all'aumento della complessità e richiedono una manutenzione continua, puoi investire di più tempo di sviluppo fin dall'inizio per creare un *livelli* oppure *layered* struttura dell'applicazione che è più facile da gestire. Per implementare questa architettura, il livello di presentazione separare dal livello della logica di business (BLL) e il livello di accesso ai dati (DAL). Un modo per implementare questa struttura consiste nell'utilizzare il `ObjectDataSource` invece di controllare il `EntityDataSource` controllo. Quando si usa la `ObjectDataSource` (controllo), si implementa il proprio codice di accesso ai dati e quindi richiamarlo in *aspx* pagine usando un controllo dotato di molte delle stesse funzionalità degli altri controlli origine dati. Ciò consente di combinare i vantaggi di un approccio a più livelli con i vantaggi dell'uso di un controllo Web Form per l'accesso ai dati.

Il `ObjectDataSource` controllo ti offre una maggiore flessibilità in anche altri modi. Dal momento che si scrittura il proprio codice di accesso ai dati, è più semplice oltre la semplice leggere, inserire, aggiornare o eliminare un tipo di entità specifiche, quali sono le attività che il `EntityDataSource` controllo progettato per l'esecuzione. Ad esempio, è possibile eseguire la registrazione di ogni volta che viene aggiornata un'entità, archiviare i dati ogni volta che viene eliminata un'entità, o automaticamente verificare e aggiornare dati correlati in base alle necessità durante l'inserimento di una riga con un valore di chiave esterna.

## <a name="business-logic-and-repository-classes"></a>La logica di business e le classi di Repository

Un `ObjectDataSource` funzionamento del controllo tramite la chiamata di una classe che viene creata. La classe include metodi che recuperano e aggiornano i dati e si deve forniscono i nomi di questi metodi per il `ObjectDataSource` controllo nel markup. Durante il rendering o l'elaborazione di postback, il `ObjectDataSource` chiama i metodi che è stato specificato.

Oltre alle operazioni CRUD di base, la classe che viene creata per l'uso con il `ObjectDataSource` controllo potrebbe essere necessario eseguire la logica di business quando il `ObjectDataSource` legge o aggiorna i dati. Ad esempio, quando si aggiorna un reparto, potrebbe essere necessario convalidare che non gli altri reparti abbiano lo stesso amministratore perché una persona non può essere amministratore di più di un reparto.

In alcuni `ObjectDataSource` documentazione, ad esempio il [Cenni preliminari sulla classe ObjectDataSource](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.aspx), il controllo chiama una classe definita un *oggetto business* che include la logica di business sia per la logica di accesso ai dati . In questa esercitazione si creerà classi separate per la logica di business e per la logica di accesso ai dati. La classe che incapsula la logica di accesso ai dati viene chiamata un *repository*. La classe per la logica di business include metodi la logica di business sia i metodi di accesso ai dati, ma il repository per eseguire attività di accesso ai dati di chiamare i metodi di accesso ai dati.

Verrà inoltre creato un livello di astrazione tra il livello BLL e DAL che facilita l'unit test automatizzati di test del livello BLL. Questo livello di astrazione viene implementato creando un'interfaccia e usando l'interfaccia quando si crea un'istanza del repository nella classe della logica di business. Questo rende possibile per ottenere la classe di logica di business con un riferimento a qualsiasi oggetto che implementa l'interfaccia del repository. Per il normale funzionamento, è fornire un oggetto di repository che funziona con Entity Framework. Per i test, è fornire un oggetto di repository che funziona con i dati archiviati in modo che è possibile gestire facilmente, ad esempio le variabili di classe definite come le raccolte.

Nella figura seguente mostra la differenza tra una classe di logica di business che include la logica di accesso ai dati senza un repository e uno che usa un repository.

[![Image05](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image1.png)

Inizierà creando pagine web in cui il `ObjectDataSource` è associato direttamente a un repository in quanto esegue solo le attività di base di accesso ai dati. Nella prossima esercitazione si crea una classe per la logica di business con la logica di convalida e associa il `ObjectDataSource` controllo invece di tale classe per la classe di repository. Si creerà anche unit test per la logica di convalida. Nella terza esercitazione di questa serie si aggiungerà l'ordinamento e filtrare le funzionalità all'applicazione.

Le pagine create in questa esercitazione usare il `Departments` set di entità del modello di dati che è stato creato nel [serie di esercitazioni con guide introduttive](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md).

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image3.png)

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image5.png)

## <a name="updating-the-database-and-the-data-model"></a>L'aggiornamento del Database e il modello di dati

Iniziare l'esercitazione da apportare due modifiche al database, che richiedono le modifiche corrispondenti nel modello di dati che è stato creato nel [Introduzione a Entity Framework e Web Form](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) esercitazioni. In uno di tali esercitazioni, sono apportate modifiche nella finestra di progettazione manualmente per sincronizzare il modello di dati con il database dopo una modifica al database. In questa esercitazione si utilizzerà la finestra di progettazione **Aggiorna modello da Database** dello strumento per aggiornare automaticamente il modello di dati.

### <a name="adding-a-relationship-to-the-database"></a>Aggiunta di una relazione al Database

In Visual Studio, aprire l'applicazione web di Contoso University è stato creato nel [Introduzione a Entity Framework e Web Form](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) serie di esercitazioni e quindi aprire il `SchoolDiagram` diagramma di database.

Se si esamina il `Department` tabella nel diagramma di database, si noterà che contiene un `Administrator` colonna. Questa colonna è una chiave esterna per il `Person` tabella, ma nessuna relazione di chiave esterna viene definita nel database. È necessario creare la relazione e aggiornare il modello di dati in modo che Entity Framework può gestire automaticamente questa relazione.

Nel diagramma di database, fare doppio clic il `Department` tabella e selezionare **relazioni**.

[![Image80](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image7.png)

Nel **Foreign Key Relationships** fare clic su casella **Add**, quindi fare clic sui puntini di sospensione per **specifica tabelle e colonne**.

[![Image81](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image10.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image9.png)

Nel **tabelle e colonne** finestra di dialogo casella, impostare la tabella della chiave primaria e campo `Person` e `PersonID`e impostare la tabella di chiave esterna e campo `Department` e `Administrator`. (Quando si esegue questa operazione, il nome della relazione verrà modificato da `FK_Department_Department` a `FK_Department_Person`.)

[![Image82](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image12.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image11.png)

Fare clic su **OK** nel **tabelle e colonne** fare clic su **Chiudi** nel **Foreign Key Relationships** casella e salvare le modifiche. Se viene chiesto se si desidera salvare il `Person` e `Department` tabelle, fare clic su **Yes**.

> [!NOTE]
> Se è stata eliminata `Person` righe corrispondenti ai dati che sono già presente nel `Administrator` colonna, non sarà in grado di salvare questa modifica. In tal caso, usare l'editor di tabella in **Esplora Server** assicurarsi che le `Administrator` valore in ogni `Department` riga contiene l'ID di un record che esista effettivamente nel `Person` tabella.
> 
> Dopo aver salvato la modifica, non sarà in grado di eliminare una riga dal `Person` tabella se tale utente è un amministratore del reparto. In un'applicazione di produzione, si fornirebbe un messaggio di errore specifico quando un vincolo di database impedisce un'operazione di eliminazione oppure è possibile specificare un'operazione di eliminazione a catena. Per un esempio di come specificare un'operazione di eliminazione a catena, vedere [Entity Framework e ASP.NET-Guida introduttiva parte 2](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2.md).


### <a name="adding-a-view-to-the-database"></a>Aggiunta di una vista nel database

Nel nuovo *Departments.aspx* pagina che si crea, si desidera fornire un elenco di riepilogo a discesa di instructors (insegnanti), con nomi nel formato "cognome, nome" in modo che gli utenti possono selezionare gli amministratori di reparto. Per renderne più semplice per farlo, si creerà una vista nel database. La visualizzazione sarà costituito semplicemente i dati necessari per l'elenco a discesa: il nome completo (correttamente formattato) e la chiave del record.

Nelle **Esplora Server**, espandere *School. mdf*, fare doppio clic sui **viste** cartella e selezionare **Aggiungi nuova vista**.

[![Image06](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image14.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image13.png)

Fare clic su **Close** quando il **Aggiungi tabella** viene visualizzata la finestra di dialogo e incollare l'istruzione SQL seguente nel riquadro SQL:

[!code-sql[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample1.sql)]

Salvare la vista come `vInstructorName`.

### <a name="updating-the-data-model"></a>L'aggiornamento del modello di dati

Nel *DAL* cartella, aprire il *SchoolModel* del file, fare doppio clic nell'area di progettazione e selezionare **Aggiorna modello da Database**.

[![Image07](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image16.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image15.png)

Nel **Scegli oggetti di Database** finestra di dialogo, seleziona la **Add** scheda e selezionare la vista appena creata.

[![Image08](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image18.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image17.png)

Scegliere **Fine**.

Nella finestra di progettazione, si noterà che lo strumento è stato creato un `vInstructorName` entità e una nuova associazione tra il `Department` e `Person` entità.

[![Image13](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image20.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image19.png)

> [!NOTE]
> Nel **Output** e **elenco errori** tasti di windows si potrebbero ricevere un messaggio di avviso che informa che lo strumento creato automaticamente un database primario per il nuovo `vInstructorName` visualizzazione. Si tratta di un comportamento previsto.


Quando si fa riferimento al nuovo `vInstructorName` entità nel codice, non si desidera utilizzare la convenzione di database apponendo il prefisso "v" minuscolo ad esso. Pertanto, verranno rinominate le entità e set di entità nel modello.

Aprire il **modellare Browser**. Noterete che `vInstructorName` elencato come un tipo di entità e una visualizzazione.

[![Image14](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image22.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image21.png)

Sotto **SchoolModel** (non **SchoolModel. Store**), fare doppio clic su **vInstructorName** e selezionare **proprietà**. Nel **delle proprietà** finestra Modifica il **nome** proprietà su "InstructorName" e modificare il **nome Set entità** proprietà su "InstructorNames".

[![Image15](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image24.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image23.png)

Salvare e chiudere il modello di dati e quindi ricompilare il progetto.

## <a name="using-a-repository-class-and-an-objectdatasource-control"></a>Utilizzo di una classe di Repository e un controllo ObjectDataSource

Creare un nuovo file di classe nel *DAL* cartella, denominarla *SchoolRepository.cs*e sostituire il codice esistente con il codice seguente:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample2.cs)]

Questo codice fornisce un'unica `GetDepartments` metodo che restituisce tutte le entità nel `Departments` set di entità. Poiché è certo che si accede il `Person` proprietà di navigazione per ogni riga restituita, si specifica il caricamento eager per tale proprietà utilizzando il `Include` (metodo). La classe implementa anche il `IDisposable` interfaccia per garantire che la connessione al database viene rilasciata quando viene eliminato l'oggetto.

> [!NOTE]
> Una procedura comune consiste nel creare una classe di repository per ogni tipo di entità. In questa esercitazione viene usata una classe di repository per più tipi di entità. Per altre informazioni sul modello di repository, vedere i post in [blog del team di Entity Framework](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx) e [blog di Julie](http://thedatafarm.com/blog/data-access/agile-ef4-repository-part-3-fine-tuning-the-repository/).


Il `GetDepartments` metodo restituisce un `IEnumerable` oggetto anziché un' `IQueryable` oggetto per garantire che la raccolta restituita è utilizzabile anche dopo aver eliminato l'oggetto repository stesso. Un `IQueryable` oggetto può causare l'accesso al database ogni volta che è possibile accedervi, ma l'oggetto repository può essere eliminato entro l'ora di un controllo con associazione a dati tenta di eseguire il rendering dei dati. È possibile restituire un altro tipo di raccolta, ad esempio un `IList` dell'oggetto anziché un `IEnumerable` oggetto. Tuttavia, restituendo un' `IEnumerable` oggetto assicura che è possibile eseguire le attività di elaborazione tipica elenco di sola lettura, ad esempio `foreach` cicli e query LINQ, ma è Impossibile aggiungere o rimuovere gli elementi nella raccolta, che potrebbe implicare che sarebbero stati tali modifiche resi persistenti nel database.

Creare un *Departments.aspx* pagina che utilizza il *Site* pagina master e aggiungere il markup seguente nel `Content` controllo denominato `Content2`:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample3.aspx)]

Questo codice crea un' `ObjectDataSource` controllo che usa la classe di repository appena creato, e un `GridView` controllo per visualizzare i dati. Il `GridView` specifica di controllo **modificare** e **eliminare** comandi, ma non sono stati aggiunti per supportarli ancora codice.

Utilizzano diverse colonne `DynamicField` controlla in modo da poter usufruire della funzionalità di formattazione e convalida automatica dei dati. Per questo scopo, è necessario chiamare il `EnableDynamicData` metodo nella `Page_Init` gestore dell'evento. (`DynamicControl` controlli non vengono usati nel `Administrator` campo perché non funzionano con le proprietà di navigazione.)

Il `Vertical-Align="Top"` attributi verranno diventano importanti in un secondo momento quando si aggiunge una colonna che nidificato `GridView` controllo alla griglia.

Aprire il *Departments.aspx.cs* del file e aggiungere quanto segue `using` istruzione:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample4.cs)]

Quindi aggiungere il gestore seguente per la pagina `Init` evento:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample5.cs)]

Nel *DAL* cartella, creare un nuovo file di classe denominato *Department.cs* e sostituire il codice esistente con il codice seguente:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample6.cs)]

Questo codice aggiunge i metadati nel modello di dati. Specifica che il `Budget` proprietà del `Department` entità rappresenta in effetti valuta anche se il tipo di dati è `Decimal`, e specifica che il valore deve essere compreso tra 0 e $1,000,000.00. Specifica inoltre che il `StartDate` proprietà deve essere formattata come una data nel formato mm/gg/aaaa.

Eseguire la *Departments.aspx* pagina.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image26.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image25.png)

Si noti che anche se non è stato specificato in una stringa di formato il *Departments.aspx* markup della pagina per il **Budget** oppure **data di inizio** predefinito di colonne, valuta e date è stata applicata la formattazione a essi dal `DynamicField` controlla, utilizzando i metadati forniti nel *Department.cs* file.

## <a name="adding-insert-and-delete-functionality"></a>Aggiunta di inserimento e la funzionalità di eliminazione

Aprire *SchoolRepository.cs*, aggiungere il codice seguente per creare un `Insert` metodo e un `Delete` (metodo). Il codice include anche un metodo denominato `GenerateDepartmentID` che calcola il valore della chiave record disponibile successivo per l'uso dal `Insert` (metodo). Questa operazione è necessaria perché il database non è configurato per questa operazione automaticamente per calcolare il `Department` tabella.

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample7.cs)]

### <a name="the-attach-method"></a>Il metodo di collegamento

Il `DeleteDepartment` chiamate al metodo il `Attach` metodo per ristabilire il collegamento che viene mantenuto nel gestore di stato oggetto di contesto dell'oggetto tra l'entità in memoria e il database di riga rappresenta. Ciò deve precedere le chiamate al metodo il `SaveChanges` (metodo).

Il termine *contesto dell'oggetto* fa riferimento alla classe che deriva da Entity Framework il `ObjectContext` classi utilizzabili per accedere ai set di entità e le entità. Nel codice per questo progetto, la classe è denominata `SchoolEntities`, e un'istanza è sempre denominata `context`. Contesto dell'oggetto *di gestione dello stato degli oggetti* è una classe che deriva dal `ObjectStateManager` classe. Contattare l'oggetto usa la gestione dello stato degli oggetti per archiviare gli oggetti entità e di tenere traccia se ognuno di essi è sincronizzato con la riga corrispondente nella tabella o le righe nel database.

Quando si legge un'entità, il contesto dell'oggetto viene memorizzato nel gestore di stato oggetto e consente di determinare se tale rappresentazione dell'oggetto è sincronizzato con il database. Ad esempio, se si modifica un valore della proprietà, viene impostato un flag per indicare che la proprietà che è stata modificata non è più sincronizzata con il database. Quando si chiama quindi il `SaveChanges` metodo, il contesto dell'oggetto individua le operazioni da eseguire nel database perché l'oggetto stato in grado di indicare esattamente ciò che è diverso tra lo stato corrente dell'entità e lo stato del database.

Tuttavia, questo processo in genere non funziona in un'applicazione web, perché l'istanza contesto dell'oggetto che legge un'entità, insieme a tutti gli elementi nel relativo gestore di stato di oggetto, viene eliminato dopo il rendering di una pagina. Istanza dell'oggetto di contesto che debba applicare le modifiche è una nuova istanza che viene creata un'istanza per l'elaborazione di postback. Nel caso del `DeleteDepartment` metodo, il `ObjectDataSource` controllo crea nuovamente la versione originale dell'entità per l'utente dai valori nello stato di visualizzazione, ma ciò ricreato `Department` entità non esiste nel gestore degli stati di oggetti. Se è stato chiamato il `DeleteObject` metodo su questa entità creata di nuovo, la chiamata avrà esito negativo perché il contesto dell'oggetto non sa se l'entità è sincronizzato con il database. Tuttavia, la chiamata di `Attach` metodo ristabilisce di rilevamento stesso tra ricreate entità e i valori nel database che è stato originariamente eseguito automaticamente quando l'entità è stata letta in un'istanza precedente del contesto dell'oggetto.

Vi sono casi quando non si desidera che il contesto dell'oggetto per tenere traccia delle entità nel gestore degli stati di oggetti, ed è possibile impostare flag per evitare che tale operazione. Nelle esercitazioni successive di questa serie sono riportati alcuni esempi di questo oggetto.

### <a name="the-savechanges-method"></a>Il metodo SaveChanges

Questa classe di repository semplice illustra i principi di base di come eseguire operazioni CRUD. In questo esempio, il `SaveChanges` metodo viene chiamato immediatamente dopo ogni aggiornamento. In un'applicazione di produzione è possibile chiamare il `SaveChanges` metodo da un metodo separato per fornire maggiore controllo sulla quando viene aggiornato il database. (Alla fine della prossima esercitazione è possibile trovare un collegamento a un white paper che illustra lo schema unit of work che è un approccio per coordinare gli aggiornamenti correlati.) Si noti inoltre che nell'esempio di `DeleteDepartment` (metodo) non include codice per gestire i conflitti di concorrenza; verrà aggiunto codice per eseguire questa operazione in un'esercitazione successiva di questa serie.

### <a name="retrieving-instructor-names-to-select-when-inserting"></a>Recuperare i nomi dei docenti per selezionare durante l'inserimento

Gli utenti devono essere in grado di selezionare un amministratore da un elenco degli insegnanti in un elenco a discesa durante la creazione di nuovi reparti. Pertanto, aggiungere il codice seguente al *SchoolRepository.cs* per creare un metodo per recuperare l'elenco degli insegnanti con la vista creata in precedenza:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample8.cs)]

### <a name="creating-a-page-for-inserting-departments"></a>Creazione di una pagina per l'inserimento di reparti

Creare un *DepartmentsAdd.aspx* pagina che utilizza il *Site* pagina e aggiungere il markup seguente nel `Content` controllo denominato `Content2`:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample9.aspx)]

Questo codice crea due `ObjectDataSource` controlli, uno per l'inserimento di nuove `Department` entità e uno per recuperare i nomi dei docenti per il `DropDownList` controllo utilizzato per la selezione agli amministratori di reparto. Crea il markup una `DetailsView` per immettere nuovi reparti che specifica un gestore per il controllo di controllo `ItemInserting` eventi in modo che sia possibile impostare il `Administrator` valore di chiave esterna. Alla fine è un `ValidationSummary` controllo per visualizzare i messaggi di errore.

Aprire *DepartmentsAdd.aspx.cs* e aggiungere quanto segue `using` istruzione:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample10.cs)]

Aggiungere i metodi e la variabile di classe seguente:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample11.cs)]

Il `Page_Init` metodo abilita la funzionalità di Dynamic Data. Il gestore per il `DropDownList` del controllo `Init` evento consente di salvare un riferimento al controllo e il gestore per il `DetailsView` del controllo `Inserting` evento usa tale riferimento per ottenere il `PersonID` valore l'insegnante selezionato e l'aggiornamento il `Administrator` proprietà di chiave esterna del `Department` entità.

Esecuzione della pagina, aggiungere le informazioni per un nuovo reparto e quindi scegliere il **Inserisci** collegamento.

[![Image04](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image28.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image27.png)

Immettere i valori per un altro nuovo reparto. Immettere un numero maggiore di 1,000,000.00 nel **Budget** campo e pressione di tab per il campo successivo. Viene visualizzato un asterisco nel campo e se si posiziona il puntatore del mouse su di esso, noterete il messaggio di errore immessi nei metadati per il campo.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image30.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image29.png)

Fare clic su **inserire**, e viene visualizzato il messaggio di errore visualizzato per il `ValidationSummary` controllo nella parte inferiore della pagina.

[![Image12](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image32.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image31.png)

Quindi, chiudere il browser e aprire il *Departments.aspx* pagina. Aggiungere funzionalità di eliminazione per il *Departments.aspx* pagina mediante l'aggiunta di un `DeleteMethod` dell'attributo il `ObjectDataSource` (controllo) e un `DataKeyNames` dell'attributo di `GridView` controllo. I tag di apertura per questi controlli saranno ora simile all'esempio seguente:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample12.aspx)]

Eseguire la pagina.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image34.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image33.png)

Eliminare il reparto è stato aggiunto durante l'esecuzione di *DepartmentsAdd.aspx* pagina.

## <a name="adding-update-functionality"></a>Aggiunta di funzionalità di aggiornamento

Aprire *SchoolRepository.cs* e aggiungere quanto segue `Update` metodo:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample13.cs)]

Quando fa clic su **Update** nel *Departments.aspx* pagina, il `ObjectDataSource` controllo crea due `Department` entità da passare al `UpdateDepartment` (metodo). Una contiene i valori originali che sono stati archiviati nello stato di visualizzazione e l'altro contenente i nuovi valori immessi nel `GridView` controllo. Il codice nel `UpdateDepartment` metodo passa il `Department` entità con i valori originali per il `Attach` metodo per stabilire la tracciabilità tra l'entità e ciò che si trova nel database. Quindi il codice passa il `Department` entità con i nuovi valori per il `ApplyCurrentValues` (metodo). Il contesto dell'oggetto confronta i valori vecchi e nuovi. Se un nuovo valore è diverso da un valore precedente, il contesto dell'oggetto viene modificato il valore della proprietà. Il `SaveChanges` metodo aggiorna quindi solo le colonne modificate nel database. (Tuttavia, se la funzione di aggiornamento per questa entità è stato eseguito il mapping a una stored procedure, l'intera riga verrà aggiornata indipendentemente da quali colonne sono state modificate.)

Aprire il *Departments.aspx* file e aggiungere i seguenti attributi per il `DepartmentsObjectDataSource` controllo:

- `UpdateMethod="UpdateDepartment"`
- `ConflictDetection="CompareAllValues"`   
 Questo cause per i precedenti valori archiviati in stato di visualizzazione in modo che essi possano essere confrontati con i nuovi valori nel `Update` (metodo).
- `OldValuesParameterFormatString="orig{0}"`   
 Ciò indica al controllo che è il nome del parametro originale del valori `origDepartment` .

Il markup per il tag di apertura di `ObjectDataSource` controllo sarà ora simile all'esempio seguente:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample14.aspx)]

Aggiungere un `OnRowUpdating="DepartmentsGridView_RowUpdating"` dell'attributo di `GridView` controllo. Verrà usato per impostare il `Administrator` valore della proprietà in base alla riga a cui l'utente seleziona in un elenco a discesa. Il `GridView` ora tag di apertura è simile al seguente:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample15.aspx)]

Aggiungere un `EditItemTemplate` di controllo per il `Administrator` colonna il `GridView` controllare, subito dopo il `ItemTemplate` controllo per la colonna:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample16.aspx)]

Ciò `EditItemTemplate` è simile al controllo il `InsertItemTemplate` controllare nel *DepartmentsAdd.aspx* pagina. La differenza è che il valore iniziale del controllo è impostato tramite il `SelectedValue` attributo.

Prima di `GridView` controllo, aggiungere un `ValidationSummary` controllare come è stato fatto *DepartmentsAdd.aspx* pagina.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample17.aspx)]

Aprire *Departments.aspx.cs* immediatamente dopo la dichiarazione di classe parziale, aggiungere il codice seguente per creare un campo privato per fare riferimento il `DropDownList` controllo:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample18.cs)]

Quindi aggiungere i gestori per il `DropDownList` del controllo `Init` evento e il `GridView` del controllo `RowUpdating` evento:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample19.cs)]

Il gestore per il `Init` evento consente di salvare un riferimento al `DropDownList` controllo nel campo della classe. Il gestore per il `RowUpdating` evento utilizza il riferimento per ottenere il valore specificato dall'utente e metterle in pratica per la `Administrator` proprietà del `Department` entità.

Usare il *DepartmentsAdd.aspx* pagina per aggiungere un nuovo reparto, quindi eseguire il *Departments.aspx* e fare clic su **Edit** sulla riga che è stato aggiunto.

> [!NOTE]
> Non sarà in grado di modificare le righe che non è stato aggiunto (vale a dire che erano già presenti nel database), a causa di dati non validi nel database. gli amministratori per le righe che sono stati creati con il database sono gli studenti. Se si prova a modificare uno di essi, si otterrà una pagina di errore che segnala un errore, ad esempio `'InstructorsDropDownList' has a SelectedValue which is invalid because it does not exist in the list of items.`


[![Image10](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image36.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image35.png)

Se si immette un valore non valido **Budget** amount e quindi fare clic su **Update**, asterisco stesso e il messaggio di errore che si è visto nella vengono visualizzati i *Departments.aspx* pagina.

Modificare un valore del campo o selezionare un altro amministratore e fare clic su **Update**. Viene visualizzata la modifica.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image38.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image37.png)

In questo argomento completa l'introduzione all'uso del `ObjectDataSource` controllo per CRUD di base (creare, leggere, aggiornare ed eliminare) le operazioni con Entity Framework. È stata creata una semplice applicazione a più livelli, ma il livello di logica di business è ancora strettamente associato a livello di accesso ai dati, che rende più difficile il testing unità automatizzato. Nell'esercitazione verrà illustrato come implementare il modello di repository per facilitare il testing unità.

> [!div class="step-by-step"]
> [avanti](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
