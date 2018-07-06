---
uid: web-forms/overview/data-access/working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs
title: Wrapping delle modifiche al Database in una transazione (c#) | Microsoft Docs
author: rick-anderson
description: Questa esercitazione è la prima delle quattro proprietà che esamina l'aggiornamento, eliminazione e inserimento batch di dati. In questa esercitazione viene illustrato come consentono le transazioni di database...
ms.author: aspnetcontent
ms.date: 06/26/2007
ms.assetid: b45fede3-c53a-4ea1-824b-20200808dbae
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs
msc.type: authoredcontent
ms.openlocfilehash: a87ba758abd6b3e89be4f5aa64d658b734f99d9e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37810929"
---
<a name="wrapping-database-modifications-within-a-transaction-c"></a>Wrapping delle modifiche al Database interno di una transazione (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare il codice](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_63_CS.zip) o [Scarica il PDF](wrapping-database-modifications-within-a-transaction-cs/_static/datatutorial63cs1.pdf)

> Questa esercitazione è la prima delle quattro proprietà che esamina l'aggiornamento, eliminazione e inserimento batch di dati. In questa esercitazione viene illustrato come le transazioni di database consentono modifiche batch come operazione atomica, che assicura che tutti i passaggi abbia esito positivo o esito negativo di tutti i passaggi da eseguire.


## <a name="introduction"></a>Introduzione

Come abbiamo visto inizia con la [una panoramica di inserimento, aggiornamento ed eliminazione di dati](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) esercitazione GridView fornisce il supporto incorporato per la modifica a livello di riga e l'eliminazione. Con pochi clic del mouse è possibile creare un'interfaccia di modifica di dati avanzate senza scrivere una riga di codice, purché sono contenuti con la modifica ed eliminazione per ogni riga. Tuttavia, in alcuni scenari non sono sufficiente e dobbiamo fornire agli utenti la possibilità di modificare o eliminare un batch di record.

Ad esempio, basato sul web più client di posta elettronica usare una griglia per visualizzare l'elenco di ogni messaggio in cui ogni riga include una casella di controllo insieme alle informazioni s messaggio di posta elettronica (oggetto, mittente e così via). Questa interfaccia consente all'utente di eliminare più messaggi selezionandole e quindi facendo clic su un pulsante Elimina messaggi selezionati. Un batch di interfaccia di modifica è ideale nelle situazioni in cui gli utenti modificare comunemente molti record diversi. Anziché richiedere all'utente di fare clic su Modifica, apportare le modifiche necessarie e quindi fare clic su Aggiorna per ogni record che deve essere modificato, un batch di interfaccia di modifica viene eseguito il rendering di ogni riga con l'interfaccia di modifica. L'utente può modificare rapidamente il set di righe che devono essere modificati e quindi salvare tali modifiche, fare clic su un pulsante Aggiorna tutto. In questa serie di esercitazioni pratiche verrà esaminato come creare interfacce per l'inserimento, modifica ed eliminazione di batch di dati.

Quando si eseguono operazioni batch s importante per determinare se è possibile per alcune delle operazioni nel batch abbia esito positivo mentre altre non riuscire. Prendere in considerazione un batch di eliminazione interfaccia - cosa dovrebbe accadere se il primo record selezionato viene eliminato correttamente, ma il secondo ha esito negativo, ad esempio, a causa di una violazione di vincolo di chiave esterna? L'eliminazione di record s prima eseguito il rollback o è accettabile per il primo record di rimanere eliminato?

Se si desidera che l'operazione batch per essere considerato come un [operazione atomica](http://en.wikipedia.org/wiki/Atomic_operation), uno in cui sia tutti i passaggi abbia esito positivo o tutti i passaggi di esito negativo, quindi deve essere ampliata per includere il supporto per il livello di accesso ai dati [database le transazioni](http://en.wikipedia.org/wiki/Database_transaction). Le transazioni del database garantiscono atomicità per il set di `INSERT`, `UPDATE`, e `DELETE` istruzioni eseguite in quello della transazione e sono una funzionalità supportata da tutti i sistemi di database moderni la maggior parte delle.

In questa esercitazione verrà esaminato come estendere per utilizzare le transazioni di database. Esercitazioni successive verranno esaminate le pagine web implementazione per batch di inserimento, aggiornamento ed eliminazione di interfacce. Introduzione a ti permettono di s.

> [!NOTE]
> Quando si modificano i dati in una transazione di batch, atomicità non è sempre necessario. In alcuni scenari, può essere accettabile avere alcune modifiche ai dati abbia esito positivo e ad altri utenti nello stesso batch hanno esito negativo, ad esempio, quando l'eliminazione di un set di messaggi di posta elettronica da un client di posta elettronica basato sul web. Se s è un database errore durante l'esecuzione dell'eliminazione del processo, è s sia accettabile che rimangano eliminati i record elaborati senza errori. In questi casi, DAL non devono essere modificate per supportare le transazioni di database. Esistono altri batch operazione scenari, tuttavia, in cui è fondamentale atomicità. Quando un cliente suo fondi da un conto bancario a altra, devono essere eseguite due operazioni: i fondi devono essere dedotto dal primo account e quindi aggiunti al secondo. Anche se la banca potrebbe non tenere il primo passaggio abbia esito positivo, ma il secondo passaggio esito negativo, i clienti comprensibilmente sarebbe contrariati. Ti invitiamo a eseguire questa esercitazione e implementare i miglioramenti apportati al livello per supportare le transazioni di database, anche se non si prevede di usarli nel batch di inserimento, aggiornamento ed eliminazione è creando le esercitazioni seguenti mostrano tre interfacce.


## <a name="an-overview-of-transactions"></a>Una panoramica delle transazioni

La maggior parte dei database includono il supporto per *transazioni*, che consentono di essere raggruppati in una singola unità logica di lavoro più comandi di database. I comandi di database che costituiscono una transazione sono necessariamente essere atomici, vale a dire che tutti i comandi avrà esito negativo o che sia tutto avrà esito positivo.

In generale, le transazioni vengono implementate tramite le istruzioni SQL usando il modello seguente:

1. Indicare l'inizio di una transazione.
2. Eseguire le istruzioni SQL che costituiscono la transazione.
3. Se si verifica un errore in una delle istruzioni dal passaggio 2, eseguire il rollback della transazione.
4. Se tutte le istruzioni del passaggio 2 viene completata senza errori, eseguire il commit della transazione.

Le istruzioni SQL utilizzate per creare, eseguire il commit e il rollback della transazione può essere immessi manualmente quando la scrittura di script SQL o la creazione di stored procedure o tramite a livello di codice implica l'utilizzo di ADO.NET o le classi dei [ `System.Transactions` spazio dei nomi](https://msdn.microsoft.com/library/system.transactions.aspx). In questa esercitazione che verranno esaminate solo la gestione delle transazioni tramite ADO.NET. In un'esercitazione futura esamineremo come utilizzare le stored procedure nel livello di accesso ai dati, a quel punto esamineremo qui le istruzioni SQL per la creazione, eseguire il rollback e il commit delle transazioni. Nel frattempo, consultare [gestione delle transazioni in Stored procedure di SQL Server](http://www.4guysfromrolla.com/webtech/080305-1.shtml) per altre informazioni.

> [!NOTE]
> Il [ `TransactionScope` classe](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) nel `System.Transactions` dello spazio dei nomi consente agli sviluppatori di eseguire a livello di codice il wrapping di una serie di istruzioni all'interno dell'ambito di una transazione e include il supporto per le transazioni complesse che coinvolgono più origini, ad esempio due database diversi o tipi eterogenei di archivi dati, ad esempio un database Microsoft SQL Server, un database Oracle e un servizio Web. Ho ve deciso di utilizzare le transazioni di ADO.NET per questa esercitazione anziché il `TransactionScope` classe poiché ADO.NET è più specifico per le transazioni di database e, in molti casi, è molto inferiore di risorse. Inoltre, in determinati scenari il `TransactionScope` classe utilizza Microsoft Distributed Transaction Coordinator (MSDTC). I problemi di prestazioni, implementazione e configurazione di MSDTC circostante rende un argomento piuttosto specializzato e avanzato e rientra nell'ambito di queste esercitazioni.


Quando si usa il provider SqlClient in ADO.NET, le transazioni vengono avviate tramite una chiamata per il [ `SqlConnection` classe](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) s [ `BeginTransaction` metodo](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.begintransaction.aspx), che restituisce un [ `SqlTransaction` oggetto](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.aspx). Le istruzioni di modifica dei dati che composizione della transazione vengono posizionate all'interno di un `try...catch` blocco. Se si verifica un errore in un'istruzione nella `try` bloccare, trasferimenti di esecuzione per il `catch` blocco in cui può essere rollback della transazione tramite il `SqlTransaction` oggetto s [ `Rollback` (metodo)](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.rollback.aspx). Se tutte le istruzioni completate correttamente, una chiamata ai `SqlTransaction` oggetto s [ `Commit` metodo](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.commit.aspx) alla fine del `try` blocco esegue il commit della transazione. Il frammento di codice seguente illustra il modello. Visualizzare [mantenere la coerenza dei Database con transazioni](http://aspnet.4guysfromrolla.com/articles/072705-1.aspx) per aggiuntiva sintassi ed esempi di utilizzo delle transazioni con ADO.NET.


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample1.cs)]

Per impostazione predefinita, gli oggetti TableAdapter in un DataSet tipizzato non utilizzare transazioni. Per fornire il supporto per le transazioni è necessario aumentare le classi TableAdapter in modo da includere metodi aggiuntivi che usano il modello precedente per eseguire una serie di istruzioni di modifica dei dati all'interno dell'ambito di una transazione. Nel passaggio 2 vedremo come usare le classi parziali per aggiungere tali metodi.

## <a name="step-1-creating-the-working-with-batched-data-web-pages"></a>Passaggio 1: Creazione di lavoro con le pagine Web i dati in batch

Prima di iniziare a esplorare come migliorare per supportare le transazioni di database, lasciare s prima di tutto si consiglia di creare le pagine web ASP.NET che è necessario per questa esercitazione e i tre seguenti. Iniziare aggiungendo una nuova cartella denominata `BatchData` e quindi aggiungere le seguenti pagine ASP.NET, associando ogni pagina con il `Site.master` pagina master.

- `Default.aspx`
- `Transactions.aspx`
- `BatchUpdate.aspx`
- `BatchDelete.aspx`
- `BatchInsert.aspx`


![Aggiungere le pagine ASP.NET per le esercitazioni relative alla SqlDataSource](wrapping-database-modifications-within-a-transaction-cs/_static/image1.gif)

**Figura 1**: aggiungere le pagine ASP.NET per le esercitazioni relative alla SqlDataSource


Come con le altre cartelle `Default.aspx` userà la `SectionLevelTutorialListing.ascx` controllo utente per elencare le esercitazioni nella relativa sezione. Pertanto, aggiungere questo controllo utente da `Default.aspx` trascinandolo da Esplora soluzioni nella pagina di visualizzazione della struttura s.


[![Aggiungere il controllo utente sectionleveltutoriallisting. ascx a default. aspx](wrapping-database-modifications-within-a-transaction-cs/_static/image2.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image1.png)

**Figura 2**: aggiungere i `SectionLevelTutorialListing.ascx` controllo utente da `Default.aspx` ([fare clic per visualizzare l'immagine con dimensioni normali](wrapping-database-modifications-within-a-transaction-cs/_static/image2.png))


Infine, aggiungere questi quattro pagine come voci per il `Web.sitemap` file. In particolare, aggiungere il markup seguente dopo la personalizzazione della mappa del sito `<siteMapNode>`:


[!code-xml[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample2.xml)]

Dopo aver aggiornato `Web.sitemap`, si consiglia di visualizzare il sito Web di esercitazioni tramite un browser. Il menu a sinistra ora include elementi per il funzionamento con le esercitazioni di dati in batch.


![Mappa del sito include ora voci per l'uso con le esercitazioni di dati in batch](wrapping-database-modifications-within-a-transaction-cs/_static/image3.gif)

**Figura 3**: la mappa del sito include ora voci per l'uso con le esercitazioni di dati in batch


## <a name="step-2-updating-the-data-access-layer-to-support-database-transactions"></a>Passaggio 2: Aggiornare il livello di accesso ai dati per supportare le transazioni di Database

Come descritto nella prima esercitazione [creazione di un livello di accesso ai dati](../introduction/creating-a-data-access-layer-cs.md), il set di dati tipizzato nel nostro DAL è costituito da oggetti DataTable e gli oggetti TableAdapter. Oggetti DataTable contengono i dati durante gli oggetti TableAdapter forniscono la funzionalità per leggere i dati dal database in oggetti DataTable, per aggiornare il database con le modifiche apportate agli oggetti DataTable e così via. È importante ricordare che gli oggetti TableAdapter forniscono due modelli per l'aggiornamento dati, noti come aggiornamento Batch e DB-Direct. Con il modello di aggiornamento Batch, l'oggetto TableAdapter viene passato un set di dati, DataTable o raccolta di DataRow. Questi dati viene enumerati e per ciascuno inserito, modificato o eliminato righe, il `InsertCommand`, `UpdateCommand`, o `DeleteCommand` viene eseguita. Con il modello di DB-Direct, TableAdapter invece passato i valori delle colonne necessarie per l'inserimento, aggiornamento o eliminazione di un singolo record. Il metodo diretto di database di modello utilizza quindi tali valori passati nella esecuzione appropriata `InsertCommand`, `UpdateCommand`, o `DeleteCommand` istruzione.

Indipendentemente dal modello di aggiornamento utilizzato, i metodi TableAdapter generati automaticamente non utilizzano transazioni. Per impostazione predefinita ogni insert, update o delete eseguite dal TableAdapter viene considerato come una singola operazione discreta. Si supponga ad esempio il modello di database con accesso diretto viene utilizzato dal codice nel livello BLL per inserire dieci record nel database. Questo codice chiama i TableAdapter `Insert` metodo dieci volte. Se i primi cinque comandi di inserimento abbia esito positivo, ma un sesto processo ha generato un'eccezione, i primi cinque record inserito rimangono nel database. Analogamente, se il modello di aggiornamento Batch viene usato per eseguire inserimenti, aggiornamenti ed eliminazioni a inserita, righe modificate ed eliminate in un oggetto DataTable, se il primo diverse modifiche ha avuto esito positivo, ma una versione più recente si è verificato un errore, tali modifiche precedenti che completato rimangono nel database.

In determinati scenari si vuole garantire l'atomicità in una serie di modifiche. A tale scopo è necessario estendere manualmente TableAdapter mediante l'aggiunta di nuovi metodi che eseguono il `InsertCommand`, `UpdateCommand`, e `DeleteCommand` s in quello di una transazione. Nelle [creazione di un livello di accesso ai dati](../introduction/creating-a-data-access-layer-cs.md) è stata esaminata tramite [classi parziali](http://en.wikipedia.org/wiki/Partial_type) per estendere la funzionalità degli oggetti DataTable all'interno del DataSet tipizzato. Questa tecnica può essere utilizzata anche con gli oggetti TableAdapter.

Il set di dati tipizzato `Northwind.xsd` si trova nel `App_Code` cartella s `DAL` sottocartella. Creare una sottocartella nel `DAL` cartella denominata `TransactionSupport` e aggiungere un nuovo file di classe denominato `ProductsTableAdapter.TransactionSupport.cs` (vedere la figura 4). Questo file conterrà l'implementazione parziale del `ProductsTableAdapter` che include metodi per l'esecuzione di modifiche ai dati usando una transazione.


![Aggiungere una cartella denominata supporto transazioni e un File di classe denominato ProductsTableAdapter.TransactionSupport.cs](wrapping-database-modifications-within-a-transaction-cs/_static/image4.gif)

**Figura 4**: aggiungere una cartella denominata `TransactionSupport` e il File di classe `ProductsTableAdapter.TransactionSupport.cs`


Immettere il codice seguente nel `ProductsTableAdapter.TransactionSupport.cs` file:


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample3.cs)]

Il `partial` parola chiave nella dichiarazione di classe qui indica al compilatore che i membri aggiunti all'interno devono essere aggiunti al `ProductsTableAdapter` classe la `NorthwindTableAdapters` dello spazio dei nomi. Si noti il `using System.Data.SqlClient` informativa nella parte superiore del file. Poiché l'oggetto TableAdapter è stato configurato per usare il provider SqlClient, Usa internamente una [ `SqlDataAdapter` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqldataadapter.aspx) oggetto per inviare comandi al database. Di conseguenza, è necessario usare il `SqlTransaction` classe per avviare la transazione e quindi eseguirne il commit o eseguirne il rollback. Se si usa un archivio dati diverso da Microsoft SQL Server, è necessario usare il provider appropriato.

Questi metodi forniscono i blocchi predefiniti necessari per iniziare, rollback e commit di una transazione. Questi elementi sono contrassegnati `public`, consentendo loro di essere usato dall'interno di `ProductsTableAdapter`, da un'altra classe di DAL, o da un altro livello nell'architettura, ad esempio il livello BLL. `BeginTransaction` Apre i TableAdapter interno `SqlConnection` (se necessario), avvia la transazione e la assegna al `Transaction` proprietà e associa la transazione alla classe interna `SqlDataAdapter` s `SqlCommand` oggetti. `CommitTransaction` e `RollbackTransaction` chiamare il `Transaction` oggetto s `Commit` e `Rollback` metodi, rispettivamente, prima di chiudere l'oggetto interno `Connection` oggetto.

## <a name="step-3-adding-methods-to-update-and-delete-data-under-the-umbrella-of-a-transaction"></a>Passaggio 3: Aggiunta di metodi per aggiornare ed eliminare dati in quello di una transazione

Con questi metodi completati, è nuovamente pronto per aggiungere metodi a `ProductsDataTable` o il livello BLL che eseguono una serie di comandi in quello di una transazione. Il metodo seguente usa il modello di aggiornamento Batch per aggiornare un `ProductsDataTable` istanza utilizzando una transazione. Inizia una transazione chiamando il `BeginTransaction` metodo e quindi viene usato un `try...catch` blocco per eseguire le istruzioni di modifica dei dati. Se la chiamata ai `Adapter` oggetto s `Update` metodo genera un'eccezione, a cui verrà trasferiti in esecuzione il `catch` blocco in cui il rollback della transazione indietro e nuovamente generata l'eccezione. Si tenga presente che il `Update` metodo implementa il pattern di aggiornamento Batch enumerando le righe dell'oggetto fornito `ProductsDataTable` ed eseguire le necessarie `InsertCommand`, `UpdateCommand`, e `DeleteCommand` s. Se uno di questi risultati di comandi in un errore, la transazione è rollback, annullando le modifiche precedenti apportate nel corso della durata di transazione s. Dovrebbe il `Update` istruzione viene completata senza errori, viene eseguito il commit della transazione nel suo complesso.


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample4.cs)]

Aggiungere il `UpdateWithTransaction` metodo per il `ProductsTableAdapter` classe tramite la classe parziale in `ProductsTableAdapter.TransactionSupport.cs`. In alternativa, questo metodo è stato possibile aggiungere a Business Logic Layer s `ProductsBLL` classe con alcune piccole modifiche sintattiche. Vale a dire, la parola chiave in `this.BeginTransaction()`, `this.CommitTransaction()`, e `this.RollbackTransaction()` dovrà essere sostituito con `Adapter` (tenere presente che `Adapter` è il nome di una proprietà in `ProductsBLL` typu `ProductsTableAdapter`).

Il `UpdateWithTransaction` metodo viene usato il criterio di aggiornamento Batch, ma una serie di chiamate di DB-Direct può essere utilizzata anche nell'ambito di una transazione, come illustrato nel metodo seguente. Il `DeleteProductsWithTransaction` metodo accetta come input un `List<T>` typu `int`, che sono il `ProductID` s da eliminare. Il metodo avvia la transazione tramite una chiamata a `BeginTransaction` quindi il `try` block, scorrere l'elenco fornito chiama il modello di DB-Direct `Delete` metodo per ogni `ProductID` valore. Se una qualsiasi delle chiamate a `Delete` ha esito negativo, il controllo viene trasferito al `catch` blocco in cui viene eseguito il rollback di transazione e l'eccezione generata nuovamente. Se tutte le chiamate a `Delete` abbia esito positivo, quindi viene eseguito il commit delle transazioni. Aggiungere questo metodo per il `ProductsBLL` classe.


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample5.cs)]

## <a name="applying-transactions-across-multiple-tableadapters"></a>Applicazione delle transazioni tra più oggetti TableAdapter

Il codice correlate alle transazioni esaminato in questa esercitazione consente di più istruzioni con il `ProductsTableAdapter` deve essere trattato come operazione atomica. Ma cosa succede se devono essere eseguite in modo atomico più modifiche alle tabelle di database diverso? Ad esempio, quando si elimina una categoria, si potrebbe essere opportuno innanzitutto riassegnare i propri prodotti correnti e alcune altre categorie. Questi due passaggi riassegnando i prodotti e l'eliminazione della categoria devono essere eseguiti come operazione atomica. Ma il `ProductsTableAdapter` include solo i metodi per la modifica il `Products` tabella e il `CategoriesTableAdapter` include solo i metodi per la modifica di `Categories` tabella. Quindi, come una transazione può includere entrambi gli oggetti TableAdapter?

È possibile aggiungere un metodo per la `CategoriesTableAdapter` denominato `DeleteCategoryAndReassignProducts(categoryIDtoDelete, reassignToCategoryID)` e dispone di tale metodo chiama una stored procedure che riassegna i prodotti e consente di eliminare la categoria all'interno dell'ambito di una transazione definita all'interno della stored procedure. Esamineremo come iniziare, eseguire il commit e rollback transazioni nelle stored procedure in un'esercitazione futura.

Un'altra opzione consiste nel creare una classe helper in DAL che contiene il `DeleteCategoryAndReassignProducts(categoryIDtoDelete, reassignToCategoryID)` (metodo). Questo metodo crea un'istanza del `CategoriesTableAdapter` e il `ProductsTableAdapter` e quindi impostare questi due oggetti TableAdapter `Connection` delle proprietà alla stessa `SqlConnection` istanza. A quel punto, uno degli oggetti due TableAdapter dovrebbe avviare la transazione con una chiamata a `BeginTransaction`. I metodi TableAdapter per riassegnare i prodotti e l'eliminazione della categoria vengano richiamati in un `try...catch` blocco con la transazione commit o il rollback di nuovo in base alle esigenze.

## <a name="step-4-adding-theupdatewithtransactionmethod-to-the-business-logic-layer"></a>Passaggio 4: Aggiunta di`UpdateWithTransaction`metodo a livello della logica di Business

In passaggio 3 è stata aggiunta un' `UpdateWithTransaction` metodo per il `ProductsTableAdapter` nel DAL. È necessario aggiungere un metodo corrispondente per il livello BLL. Sebbene il livello di presentazione può chiamare direttamente down per richiamare il `UpdateWithTransaction` (metodo), queste esercitazioni sono strived definire un'architettura a più livelli che isola dal livello di presentazione. Pertanto, behooves per continuare a questo approccio.

Aprire il `ProductsBLL` file di classe e aggiungere un metodo denominato `UpdateWithTransaction` tale semplicemente le chiamate al metodo DAL corrispondente. Dovrebbe essere presente due nuovi metodi nella `ProductsBLL`: `UpdateWithTransaction`, che appena aggiunto, e `DeleteProductsWithTransaction`, che è stato aggiunto nel passaggio 3.


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample6.cs)]

> [!NOTE]
> Questi metodi non includono il `DataObjectMethodAttribute` attributo assegnato per la maggior parte degli altri metodi nel `ProductsBLL` classe poiché è sarà richiamare tali metodi direttamente dalle classi di code-behind di pagine ASP.NET. Si tenga presente che `DataObjectMethodAttribute` viene utilizzato per contrassegnare i metodi devono essere visualizzato in s ObjectDataSource Configura origine dati guidato e in quale scheda (SELECT, UPDATE, INSERT o DELETE). Poiché il controllo GridView non dispone di alcun supporto incorporato per la modifica o eliminazione in batch, è necessario chiamare questi metodi a livello di programmazione anziché utilizzare l'approccio senza codice dichiarativo.


## <a name="step-5-atomically-updating-database-data-from-the-presentation-layer"></a>Passaggio 5: Aggiornare in modo atomico i dati del Database dal livello di presentazione

Per illustrare l'effetto della transazione durante l'aggiornamento di un batch di record, s ti permettono di creare un'interfaccia utente che elenca tutti i prodotti in un controllo GridView e include una Web pulsante controllare che, quando si fa clic, riassegna i prodotti `CategoryID` valori. In particolare, la riassegnazione categoria stato in modo che i primi prodotti diversi vengono assegnati un valido `CategoryID` valore mentre altri sono intenzionalmente assegnato un inesistenti `CategoryID` valore. Se si tenta di aggiornare il database con un prodotto il cui `CategoryID` non corrisponde a una categoria esistente s `CategoryID`, si verificherà una violazione di vincolo di chiave esterna e verrà generata un'eccezione. Si vedrà in questo esempio è che, quando l'utilizzo di una transazione, l'eccezione generata dalla violazione di vincolo di chiave esterna causerà precedente valido `CategoryID` modifiche per eseguire il rollback. Quando non si usa una transazione, tuttavia, le modifiche alle categorie iniziale rimarranno.

Iniziare aprendo il `Transactions.aspx` nella pagina di `BatchData` cartelle e trascinare un controllo GridView dalla casella degli strumenti nella finestra di progettazione. Impostare relativi `ID` al `Products` e, dal suo smart tag, associarlo a un nuovo oggetto ObjectDataSource denominato `ProductsDataSource`. Configurare ObjectDataSource per estrarre i dati dal `ProductsBLL` classe s `GetProducts` (metodo). Questo verrà un controllo GridView di sola lettura, quindi impostare gli elenchi a discesa nell'aggiornamento, inserimento ed eliminare schede su (nessuno) e fare clic su Fine.


[![Figura 5: Configurare ObjectDataSource per usare il metodo di classe ProductsBLL s GetProducts](wrapping-database-modifications-within-a-transaction-cs/_static/image5.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image3.png)

**Figura 5**: nella figura 5: configurare ObjectDataSource per usare il `ProductsBLL` classe s `GetProducts` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](wrapping-database-modifications-within-a-transaction-cs/_static/image4.png))


[![Impostare gli elenchi a discesa nell'aggiornamento, inserimento ed eliminare schede su (nessuno)](wrapping-database-modifications-within-a-transaction-cs/_static/image6.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image5.png)

**Figura 6**: impostare l'elenco a discesa sono elencati nell'aggiornamento, inserimento ed eliminare schede su (nessuno) ([fare clic per visualizzare l'immagine con dimensioni normali](wrapping-database-modifications-within-a-transaction-cs/_static/image6.png))


Dopo aver completato la procedura guidata Configura origine dati, Visual Studio creerà BoundField e un CampoCasellaDiControllo per i campi di dati del prodotto. Rimuovere tutti questi campi, ad eccezione di `ProductID`, `ProductName`, `CategoryID`, e `CategoryName` e rinominare il `ProductName` e `CategoryName` BoundField `HeaderText` proprietà su Product e Category, rispettivamente. Nello smart tag, selezionare l'opzione attiva Paging. Dopo aver apportato queste modifiche, il markup dichiarativo s GridView e ObjectDataSource dovrebbe essere simile al seguente:


[!code-aspx[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample7.aspx)]

Successivamente, aggiungere tre controlli Web pulsante sopra il controllo GridView. Impostare il primo pulsante s proprietà Text su Aggiorna la griglia, il secondo s per modificare le categorie (con transazione) e il terzo una s per modificare le categorie (senza transazione).


[!code-aspx[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample8.aspx)]

A questo punto la visualizzazione di progettazione in Visual Studio dovrebbe essere simile allo screenshot illustrato nella figura 7.


[![La pagina contiene un controllo GridView e tre i controlli Web di pulsante](wrapping-database-modifications-within-a-transaction-cs/_static/image7.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image7.png)

**Figura 7**: la pagina contiene un GridView e tre i controlli Web pulsante ([fare clic per visualizzare l'immagine con dimensioni normali](wrapping-database-modifications-within-a-transaction-cs/_static/image8.png))


Creare i gestori eventi per ogni pulsante con i tre s `Click` eventi e usare il codice seguente:


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample9.cs)]

L'aggiornamento pulsante s `Click` gestore dell'evento riassocia semplicemente i dati a GridView mediante la chiamata di `Products` GridView s `DataBind` (metodo).

Il secondo gestore dell'evento Riassegna i prodotti `CategoryID` s e Usa il nuovo metodo di transazione da BLL quelle del database aggiornato in quello di una transazione. Si noti che ogni prodotto 1!s `CategoryID` è impostata arbitrariamente sullo stesso valore come relativo `ProductID`. Questa operazione funzionerà correttamente per la prima alcuni prodotti, poiché tali prodotti siano `ProductID` i valori che si verificano per eseguire il mapping a valido `CategoryID` s. Ma una volta il `ProductID` avvio s diventi troppo grande, questa sovrapposizione di una coincidenza `ProductID` s e `CategoryID` s non è più valido.

La terza `Click` gestore dell'evento aggiorna i prodotti `CategoryID` s allo stesso modo, ma invia l'aggiornamento al database tramite il `ProductsTableAdapter` predefiniti `Update` (metodo). Ciò `Update` metodo non esegue il wrapping della serie di comandi in una transazione, verranno mantenuti in modo che le modifiche vengono applicate prima il primo errore di violazione di vincolo di chiave esterna si è verificato.

Per illustrare questo comportamento, visitare questa pagina tramite un browser. Inizialmente si dovrebbe vedere la prima pagina di dati come illustrato nella figura 8. Successivamente, fare clic sul pulsante di modificare le categorie (con transazione). Verrà possono causare un postback e tentare di aggiornare tutti i prodotti `CategoryID` valori, ma comporta una violazione di vincolo di chiave esterna (vedere la figura 9).


[![I prodotti vengono visualizzati in un controllo GridView paginabile](wrapping-database-modifications-within-a-transaction-cs/_static/image8.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image9.png)

**Figura 8**: The prodotti vengono visualizzati in un controllo GridView paginabile ([fare clic per visualizzare l'immagine con dimensioni normali](wrapping-database-modifications-within-a-transaction-cs/_static/image10.png))


[![Riassegnare i risultati di categorie una violazione di vincolo di chiave esterna](wrapping-database-modifications-within-a-transaction-cs/_static/image9.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image11.png)

**Figura 9**: riassegnando i risultati di categorie di una violazione di vincolo di chiave esterna ([fare clic per visualizzare l'immagine con dimensioni normali](wrapping-database-modifications-within-a-transaction-cs/_static/image12.png))


Ora premo il pulsante Indietro del browser s e quindi fare clic sul pulsante Aggiorna la griglia. Dopo l'aggiornamento dei dati si dovrebbe essere lo stesso output esattamente come illustrato nella figura 8. Vale a dire, anche se alcuni prodotti `CategoryID` s erano i valori modificati per legali e aggiornate nel database, è stato eseguito il rollback quando si è verificata la violazione di vincolo di chiave esterna.

Provare subito facendo clic sul pulsante Modifica categorie (senza transazione). Ciò comporterà lo stesso errore di violazione di vincolo di chiave esterna (vedere la figura 9), ma questa volta i prodotti il cui `CategoryID` valori sono stati modificati in un legale valore verrà non rollback. Premere il pulsante Indietro del browser s e quindi il pulsante Aggiorna la griglia. Come illustrato nella figura 10, il `CategoryID` s dei primi otto prodotti sono stati riassegnati. Nella figura 8, ad esempio, registrazione di modifiche ha un `CategoryID` pari a 1, ma nella figura 10 it s è stato riassegnato a 2.


[![Alcuni prodotti CategoryID valori aggiornati mentre altri sono stati non erano](wrapping-database-modifications-within-a-transaction-cs/_static/image10.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image13.png)

**Figura 10**: alcuni prodotti `CategoryID` valori aggiornati mentre altri sono stati non erano ([fare clic per visualizzare l'immagine con dimensioni normali](wrapping-database-modifications-within-a-transaction-cs/_static/image14.png))


## <a name="summary"></a>Riepilogo

Per impostazione predefinita, i metodi di s TableAdapter non andare a capo le istruzioni di database eseguito all'interno dell'ambito di una transazione, ma con qualche operazione è possibile aggiungere metodi che verranno creato, commit e rollback di una transazione. In questa esercitazione sono stati creati tre tali metodi nel `ProductsTableAdapter` classe: `BeginTransaction`, `CommitTransaction`, e `RollbackTransaction`. È stato illustrato come utilizzare questi metodi insieme a un `try...catch` blocco per rendere una serie di istruzioni di modifica dati atomic. In particolare, è stato creato il `UpdateWithTransaction` metodo nella `ProductsTableAdapter`, che usa il modello di aggiornamento Batch per eseguire le modifiche necessarie per le righe di una classe fornita `ProductsDataTable`. Abbiamo anche aggiunto il `DeleteProductsWithTransaction` metodo per il `ProductsBLL` classe nel livello BLL, che accetta un `List` dei `ProductID` valori come input e chiama il metodo pattern di DB-Direct `Delete` per ogni `ProductID`. Entrambi i metodi per iniziare, creazione di una transazione e quindi eseguendo le istruzioni di modifica dei dati all'interno di un `try...catch` blocco. Se si verifica un'eccezione, viene eseguito il rollback della transazione, in caso contrario, viene eseguito il commit.

Passaggio 5 è stato illustrato l'effetto degli aggiornamenti di batch transazionale e aggiornamenti batch che porti a usare una transazione. Nelle successive tre esercitazioni verranno le basi di cui in questa esercitazione si basano e creare interfacce utente per l'esecuzione di aggiornamenti batch, eliminazioni e inserimenti.

Buona programmazione!

## <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Mantenere la coerenza del Database con transazioni](http://aspnet.4guysfromrolla.com/articles/072705-1.aspx)
- [Stored procedure per la gestione delle transazioni in SQL Server](http://www.4guysfromrolla.com/webtech/080305-1.shtml)
- [Operazioni più semplici: `System.Transactions`](https://blogs.msdn.com/florinlazar/archive/2004/07/23/192239.aspx)
- [TransactionScope e DataAdapter](http://andyclymer.blogspot.com/2007/01/transactionscope-and-dataadapters.html)
- [Uso delle transazioni di Database Oracle in .NET](http://www.oracle.com/technology/pub/articles/price_dbtrans_dotnet.html)

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. I revisori per questa esercitazione sono state Dave Gardner, Hilton Giesenow e Teresa Murphy. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [avanti](batch-updating-cs.md)
