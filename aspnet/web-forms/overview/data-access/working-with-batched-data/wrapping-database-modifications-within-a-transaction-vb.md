---
uid: web-forms/overview/data-access/working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb
title: Il wrapping delle modifiche del Database all'interno di una transazione (VB) | Documenti Microsoft
author: rick-anderson
description: In questa esercitazione è la prima delle quattro che esamina l'aggiornamento, eliminazione e inserimento di batch di dati. In questa esercitazione viene illustrato come cui consentire le transazioni di database...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: 7d821db5-6cbb-4b38-af14-198f9155fc82
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb
msc.type: authoredcontent
ms.openlocfilehash: 2005561755b22f5811d011bd3146853f6cd184af
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30888705"
---
<a name="wrapping-database-modifications-within-a-transaction-vb"></a>Modifiche al Database di ritorno a capo all'interno di una transazione (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare codice](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_63_VB.zip) o [Scarica il PDF](wrapping-database-modifications-within-a-transaction-vb/_static/datatutorial63vb1.pdf)

> In questa esercitazione è la prima delle quattro che esamina l'aggiornamento, eliminazione e inserimento di batch di dati. In questa esercitazione viene illustrato come le transazioni di database consentono le modifiche di batch essere eseguita come operazione atomica, che assicura che tutti i passaggi di esito positivo o esito negativo di tutti i passaggi.


## <a name="introduction"></a>Introduzione

Come abbiamo visto a partire dal [una panoramica di inserimento, aggiornamento ed eliminazione di dati](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) esercitazione GridView fornisce supporto incorporato per la modifica a livello di riga e l'eliminazione. Con pochi clic del mouse è possibile creare un'interfaccia di modifica di dati complessi senza scrivere una riga di codice, purché si è contenuto con la modifica ed eliminazione per ogni riga. Tuttavia, in determinati scenari è sufficiente ed è necessario fornire agli utenti la possibilità di modificare o eliminare un batch di record.

Ad esempio, basato sul web più client di posta elettronica utilizzare una griglia per elencare ogni messaggio in cui ogni riga include una casella di controllo insieme alle informazioni s del messaggio di posta elettronica (oggetto, mittente e così via). Questa interfaccia consente all'utente di eliminare più messaggi archiviarli e quindi facendo clic su un pulsante Elimina messaggi selezionati. Un batch di interfaccia di modifica è ideale nelle situazioni in cui gli utenti in genere modificare molti record diversi. Anziché richiedere all'utente di fare clic su Modifica, apportare la modifica e quindi fare clic su Aggiorna per ogni record che deve essere modificata, esegue il rendering di un batch di interfaccia di modifica ogni riga con l'interfaccia di modifica. L'utente può modificare rapidamente il set di righe che devono essere modificati e quindi salvare tali modifiche, fare clic su un pulsante Aggiorna tutto. In questa serie di esercitazioni si esamineranno sulla creazione di interfacce per l'inserimento, modifica ed eliminazione di batch di dati.

Durante l'esecuzione di operazioni batch è importante determinare se deve essere possibile per le operazioni nel batch abbia esito positivo mentre altre s esito negativo. Si consideri un batch di eliminazione interfaccia - cosa dovrebbe accadere se il primo record selezionato viene eliminato correttamente, ma la seconda ha esito negativo, ad esempio, a causa di una violazione di vincolo di chiave esterna? L'eliminazione di record s prima eseguito il rollback o è accettabile per il primo record di rimanere eliminato?

Se si desidera che l'operazione batch per essere considerato come un [operazione atomica](http://en.wikipedia.org/wiki/Atomic_operation), uno in cui sia tutte le istruzioni di esito positivo o tutte le operazioni avranno esito negativo, quindi deve essere ampliate per includere il supporto per il livello di accesso ai dati [database le transazioni](http://en.wikipedia.org/wiki/Database_transaction). Le transazioni di database garantiscono atomicità per il set di `INSERT`, `UPDATE`, e `DELETE` istruzioni eseguite in generico della transazione e sono una funzionalità supportata da tutti i sistemi del database moderno la maggior parte delle.

In questa esercitazione viene illustrato come estendere per utilizzare le transazioni di database. Esercitazioni successive verranno esaminate le pagine web di implementazione per batch inserimento, aggiornamento ed eliminazione di interfacce. Let s iniziare!

> [!NOTE]
> Quando si modificano i dati in una transazione di batch, atomicità non è sempre necessario. In alcuni scenari, può essere accettabile abbia esito positivo alcune modifiche di dati e altri utenti nello stesso batch esito negativo, ad esempio quando l'eliminazione di un set di messaggi di posta elettronica da un client di posta elettronica basato sul web. Se s è a metà un errore di database mediante l'eliminazione processo, è s sia accettabile che rimangano eliminati i record elaborati senza errori. In questi casi, non è necessario essere modificate per supportare le transazioni di database DAL. Esistono altri batch operazione scenari, tuttavia, in cui atomicità è essenziale. Quando un cliente proprio fondi da un conto bancario a altra, è necessario effettuare due operazioni: fondi devono essere sottratto dal primo account e quindi aggiunto al secondo. Mentre la banca può non tenere il primo passaggio esito positivo ma non il secondo passaggio, i clienti sarebbe naturalmente Conference. Ti suggeriamo di questa esercitazione e implementare i miglioramenti di per supportare le transazioni di database, anche se non si intende utilizzarli nel batch di inserimento, aggiornamento ed eliminazione è verrà creata nelle esercitazioni seguenti tre interfacce.


## <a name="an-overview-of-transactions"></a>Una panoramica delle transazioni

La maggior parte dei database includono il supporto per *transazioni*, che consente più comandi di database vengono raggruppati in una singola unità logica di lavoro. I comandi di database che costituiscono una transazione sono garantiti come atomica, vale a dire che tutti i comandi avrà esito negativo oppure tutti avrà esito positivo.

In generale, le transazioni vengono implementate tramite le istruzioni SQL usando il modello seguente:

1. Indicare l'inizio di una transazione.
2. Eseguire le istruzioni SQL che costituiscono la transazione.
3. Se è presente un errore in una delle istruzioni nel passaggio 2, il rollback della transazione.
4. Se tutte le istruzioni nel passaggio 2 viene completata senza errori, eseguire il commit della transazione.

Le istruzioni SQL utilizzate per creare, eseguire il commit e il rollback della transazione possa essere immessi manualmente durante la scrittura di script SQL o la creazione di stored procedure o a livello di codice comporta l'utilizzo di ADO.NET o le classi di [ `System.Transactions` spazio dei nomi](https://msdn.microsoft.com/library/system.transactions.aspx). In questa esercitazione che verranno esaminati solo la gestione delle transazioni mediante ADO.NET. In una futura esercitazione verranno esaminati illustrato come utilizzare stored procedure nel livello di accesso ai dati, a quel punto si esamineranno le istruzioni SQL per la creazione, rollback e il commit delle transazioni. Nel frattempo, consultare [gestione delle transazioni in Stored procedure di SQL Server](http://www.4guysfromrolla.com/webtech/080305-1.shtml) per ulteriori informazioni.

> [!NOTE]
> Il [ `TransactionScope` classe](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) nel `System.Transactions` dello spazio dei nomi consente agli sviluppatori di eseguire a livello di codice il wrapping di una serie di istruzioni all'interno dell'ambito di una transazione e include il supporto per le transazioni complesse che coinvolgono più origini, ad esempio due database diversi o anche eterogenei tipi di archivi di dati, ad esempio un database di Microsoft SQL Server, un database Oracle e un servizio Web. Si va ha deciso di utilizzare le transazioni di ADO.NET per questa esercitazione anziché il `TransactionScope` classe poiché ADO.NET è più specifico per le transazioni di database e, in molti casi, è molto inferiore di risorse. Inoltre, in determinate circostanze la `TransactionScope` classe utilizza Microsoft Distributed Transaction Coordinator (MSDTC). I problemi di configurazione, implementazione e delle prestazioni MSDTC circostante rende un argomento piuttosto specializzato e avanzato e rientra nell'ambito di queste esercitazioni.


Quando si utilizza il provider SqlClient in ADO.NET, le transazioni vengono avviate tramite una chiamata al [ `SqlConnection` classe](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) s [ `BeginTransaction` metodo](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.begintransaction.aspx), che restituisce un [ `SqlTransaction` oggetto](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.aspx). Le istruzioni di modifica di dati che la struttura della transazione vengono posizionate all'interno di un `try...catch` blocco. Se si verifica un errore in un'istruzione nel `try` blocco, i trasferimenti di esecuzione per il `catch` blocco in cui la transazione può eseguire il rollback tramite il `SqlTransaction` oggetto s [ `Rollback` metodo](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.rollback.aspx). Se tutte le istruzioni completate correttamente, una chiamata al `SqlTransaction` oggetto s [ `Commit` metodo](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.commit.aspx) alla fine del `try` blocco viene eseguito il commit della transazione. Frammento di codice seguente viene illustrato questo modello. Vedere [mantenendo la coerenza del Database con transazioni](http://aspnet.4guysfromrolla.com/articles/072705-1.aspx) per ulteriore esempi di sintassi e utilizzo delle transazioni con ADO.NET.


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample1.vb)]

Per impostazione predefinita, gli oggetti TableAdapter in un DataSet tipizzato non si utilizzano transazioni. Per fornire il supporto per le transazioni di cui è necessario estendere le classi TableAdapter per includere metodi aggiuntivi che utilizzano il modello precedente per eseguire una serie di istruzioni di modifica dei dati all'interno dell'ambito di una transazione. Nel passaggio 2 vedremo come utilizzare le classi parziali per aggiungere tali metodi.

## <a name="step-1-creating-the-working-with-batched-data-web-pages"></a>Passaggio 1: Creazione di lavoro con dati in batch le pagine Web

Prima di iniziare l'esplorazione come migliorare per supportare le transazioni di database, lasciare s prima richiedere alcuni minuti per creare le pagine web ASP.NET che è necessario per questa esercitazione e i tre che seguono. Per iniziare, aggiungere una nuova cartella denominata `BatchData` e quindi aggiungere le seguenti pagine ASP.NET, l'associazione di ogni pagina con il `Site.master` pagina master.

- `Default.aspx`
- `Transactions.aspx`
- `BatchUpdate.aspx`
- `BatchDelete.aspx`
- `BatchInsert.aspx`


![Aggiungere le pagine ASP.NET per le esercitazioni relative SqlDataSource](wrapping-database-modifications-within-a-transaction-vb/_static/image1.gif)

**Figura 1**: aggiungere le pagine ASP.NET per le esercitazioni relative alla SqlDataSource


Come con le altre cartelle, `Default.aspx` utilizzerà il `SectionLevelTutorialListing.ascx` controllo utente all'elenco di esercitazioni all'interno della sezione. Pertanto, aggiungere il controllo utente `Default.aspx` trascinandolo da Esplora soluzioni nella pagina di visualizzazione della struttura s.


[![Aggiungere il controllo utente SectionLevelTutorialListing.ascx Default.aspx](wrapping-database-modifications-within-a-transaction-vb/_static/image2.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image1.png)

**Figura 2**: aggiungere il `SectionLevelTutorialListing.ascx` controllo utente al `Default.aspx` ([fare clic per visualizzare l'immagine ingrandita](wrapping-database-modifications-within-a-transaction-vb/_static/image2.png))


Infine, aggiungere questi quattro pagine come voci per il `Web.sitemap` file. In particolare, aggiungere il markup seguente dopo la personalizzazione della mappa del sito `<siteMapNode>`:


[!code-xml[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample2.xml)]

Dopo aver aggiornato `Web.sitemap`, dedicare alcuni minuti per visualizzare il sito Web esercitazioni tramite un browser. Il menu a sinistra include elementi per l'utilizzo con le esercitazioni di dati in batch.


![Mappa del sito include ora le voci per l'utilizzo con le esercitazioni di dati in batch](wrapping-database-modifications-within-a-transaction-vb/_static/image3.gif)

**Figura 3**: mappa del sito include ora le voci per lavorare con i dati in batch esercitazioni


## <a name="step-2-updating-the-data-access-layer-to-support-database-transactions"></a>Passaggio 2: Aggiornamento a livello di accesso ai dati per supportare le transazioni di Database

Come descritto nella prima esercitazione, [la creazione di un livello di accesso ai dati](../introduction/creating-a-data-access-layer-vb.md), il set di dati tipizzato nel nostro DAL è costituito da oggetti TableAdapter e DataTable. DataTable contengono dati mentre gli oggetti TableAdapter forniscono le funzionalità per la lettura dei dati dal database in DataTable, aggiornare il database con le modifiche apportate al DataTable e così via. Tenere presente che gli oggetti TableAdapter forniscono due modelli per l'aggiornamento dati, noti come aggiornamento Batch e DB-Direct. Con il criterio di aggiornamento Batch, il TableAdapter viene passato un set di dati, DataTable o raccolta di DataRow. Questi dati viene enumerati e per ogni inseriti, modificata o eliminata una riga, il `InsertCommand`, `UpdateCommand`, o `DeleteCommand` viene eseguita. Con il criterio di DB-Direct, TableAdapter invece passato i valori delle colonne necessarie per l'inserimento, aggiornamento o eliminazione di un singolo record. Il metodo pattern DB diretto utilizza quindi tali valori passata per eseguire appropriata `InsertCommand`, `UpdateCommand`, o `DeleteCommand` istruzione.

Indipendentemente dal modello di aggiornamento utilizzato, i metodi generati automaticamente gli oggetti TableAdapter non si utilizzano transazioni. Per impostazione predefinita ogni istruzione insert, update o delete eseguite dal TableAdapter viene considerato come una singola operazione discreta. Si supponga, ad esempio, il modello di DB-Direct viene utilizzato da codice in BLL per inserire i dieci record nel database. Questo codice chiama i TableAdapter `Insert` metodo dieci volte. Se le prime cinque operazioni di inserimento esito positivo, ma quello sesto ha restituito un'eccezione, i primi cinque record inseriti rimarrebbero nel database. Analogamente, se il modello di aggiornamento Batch viene usato per eseguire inserimenti, aggiornamenti ed eliminazioni per inserito, modificato ed eliminare le righe in un oggetto DataTable, se il primo alcune modifiche ha avuto esito positivo, ma una versione più recente ha rilevato un errore, le modifiche precedenti che completamento rimarrà nel database.

In determinati scenari si desidera garantire l'atomicità in una serie di modifiche. A tale scopo è necessario estendere manualmente il TableAdapter aggiungendo nuovi metodi che eseguono il `InsertCommand`, `UpdateCommand`, e `DeleteCommand` s sotto ombrello di una transazione. In [la creazione di un livello di accesso ai dati](../introduction/creating-a-data-access-layer-vb.md) abbiamo esaminato tramite [classi parziali](http://en.wikipedia.org/wiki/Partial_type) per estendere le funzionalità di DataTable all'interno del DataSet tipizzato. Questa tecnica può essere usata anche con gli oggetti TableAdapter.

Il DataSet tipizzato `Northwind.xsd` si trova nella `App_Code` cartella s `DAL` sottocartella. Creare una sottocartella di `DAL` cartella denominata `TransactionSupport` e aggiungere un nuovo file di classe denominato `ProductsTableAdapter.TransactionSupport.vb` (vedere la figura 4). Questo file contiene l'implementazione parziale del `ProductsTableAdapter` che include metodi per l'esecuzione di modifiche ai dati utilizzando una transazione.


![Aggiungere una cartella denominata TransactionSupport e un File di classe denominato ProductsTableAdapter.TransactionSupport.vb](wrapping-database-modifications-within-a-transaction-vb/_static/image4.gif)

**Figura 4**: aggiungere una cartella denominata `TransactionSupport` e un File di classe denominato `ProductsTableAdapter.TransactionSupport.vb`


Immettere il codice seguente nel `ProductsTableAdapter.TransactionSupport.vb` file:


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample3.vb)]

Il `Partial` parola chiave nella dichiarazione di classe qui indica al compilatore che devono essere aggiunti per i membri aggiunti all'interno di `ProductsTableAdapter` classe il `NorthwindTableAdapters` dello spazio dei nomi. Si noti il `Imports System.Data.SqlClient` istruzione all'inizio del file. Poiché il TableAdapter è stato configurato per utilizzare il provider SqlClient, utilizza internamente un [ `SqlDataAdapter` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqldataadapter.aspx) oggetto per eseguire i comandi al database. Di conseguenza, è necessario utilizzare la `SqlTransaction` classe per avviare la transazione e quindi eseguirne il commit o eseguirne il rollback. Se si utilizza un archivio dati diverso da Microsoft SQL Server, è necessario utilizzare il provider appropriato.

Questi metodi forniscono i blocchi predefiniti necessari per l'avvio, rollback e commit di una transazione. Questi ultimi vengono contrassegnati `Public`, consentendo loro utilizzabile dall'interno di `ProductsTableAdapter`, da un'altra classe DAL, o da un altro livello dell'architettura della finestra, ad esempio il livello Business LOGIC. `BeginTransaction` Apre i TableAdapter interno `SqlConnection` (se necessario), avvia una transazione e lo assegna al `Transaction` proprietà e allega la transazione alla classe interna `SqlDataAdapter` s `SqlCommand` oggetti. `CommitTransaction` e `RollbackTransaction` chiamare la `Transaction` oggetto s `Commit` e `Rollback` metodi, rispettivamente, prima di chiudere interna `Connection` oggetto.

## <a name="step-3-adding-methods-to-update-and-delete-data-under-the-umbrella-of-a-transaction"></a>Passaggio 3: Aggiunta di metodi per aggiornare ed eliminare dati in generico di una transazione

Con questi metodi completati è re pronti per aggiungere metodi alla `ProductsDataTable` o il livello Business LOGIC che eseguono una serie di comandi di ombrello di una transazione. Il metodo seguente usa il modello di aggiornamento Batch per aggiornare un `ProductsDataTable` istanza utilizzando una transazione. Inizia una transazione chiamando il `BeginTransaction` (metodo) e quindi viene utilizzato un `Try...Catch` blocco per eseguire le istruzioni di modifica di dati. Se la chiamata al `Adapter` oggetto s `Update` metodo genera un'eccezione, a cui verrà trasferiti in esecuzione il `catch` blocco in cui la transazione sarà possibile eseguire il rollback e l'eccezione generata nuovamente. Tenere presente che il `Update` metodo implementa il pattern di aggiornamento Batch enumerando le righe dell'oggetto fornito `ProductsDataTable` ed eseguire i necessari `InsertCommand`, `UpdateCommand`, e `DeleteCommand` s. Se uno di questi risultati di comandi in un errore, la transazione è eseguito il rollback, annullando le modifiche precedenti apportate durante la durata della transazione s. Deve il `Update` istruzione viene completata senza errori, viene eseguito il commit della transazione nella sua interezza.


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample4.vb)]

Aggiungere il `UpdateWithTransaction` metodo il `ProductsTableAdapter` classe tramite la classe parziale in `ProductsTableAdapter.TransactionSupport.vb`. In alternativa, questo metodo è stato possibile aggiungere al livello di logica di Business s `ProductsBLL` classe con alcune piccole modifiche sintattiche. In particolare, la parola chiave `Me` in `Me.BeginTransaction()`, `Me.CommitTransaction()`, e `Me.RollbackTransaction()` dovrà essere sostituito con `Adapter` (tenere presente che `Adapter` è il nome di una proprietà in `ProductsBLL` di tipo `ProductsTableAdapter`).

Il `UpdateWithTransaction` metodo utilizza il modello di aggiornamento Batch, ma una serie di chiamate di DB-Direct consente anche all'interno dell'ambito di una transazione, come illustrato nel metodo seguente. Il `DeleteProductsWithTransaction` metodo accetta come input un `List(Of T)` di tipo `Integer`, che sono il `ProductID` s da eliminare. Il metodo avvia la transazione tramite una chiamata a `BeginTransaction` quindi il `Try` di blocco, scorrere l'elenco fornito, chiamare il modello di DB-Direct `Delete` metodo per ogni `ProductID` valore. Se una qualsiasi delle chiamate a `Delete` ha esito negativo, il controllo viene trasferito per il `Catch` blocco in cui viene eseguito il rollback di transazione e l'eccezione generata nuovamente. Se tutte le chiamate a `Delete` abbia esito positivo, quindi viene eseguito il commit delle transazioni. Questo metodo per aggiungere la `ProductsBLL` classe.


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample5.vb)]

## <a name="applying-transactions-across-multiple-tableadapters"></a>L'applicazione delle transazioni tra più oggetti TableAdapter

Il codice relativo alla transazione esaminato in questa esercitazione consente di più istruzioni il `ProductsTableAdapter` deve essere trattato come operazione atomica. Ma cosa accade se devono essere eseguite in modo atomico più le modifiche alle tabelle di database diverso? Ad esempio, quando si elimina una categoria, innanzitutto potrebbe essere opportuno riassegnare i propri prodotti corrente e alcune altre categorie. Questi due passaggi riassegnazione i prodotti e l'eliminazione della categoria devono essere eseguiti come operazione atomica. Ma la `ProductsTableAdapter` include solo i metodi per la modifica di `Products` tabella e `CategoriesTableAdapter` include solo i metodi per la modifica il `Categories` tabella. In che modo può includere entrambi gli oggetti TableAdapter in una transazione?

È possibile aggiungere un metodo per il `CategoriesTableAdapter` denominato `DeleteCategoryAndReassignProducts(categoryIDtoDelete, reassignToCategoryID)` di tale metodo richiama una stored procedure che riassegna i prodotti e consente di eliminare la categoria all'interno dell'ambito di una transazione definita all'interno della stored procedure. Verrà esaminato come iniziare, eseguire il commit e rollback di transazioni nelle stored procedure in un'esercitazione future.

Un'altra opzione consiste nel creare una classe helper nel DAL che contiene il `DeleteCategoryAndReassignProducts(categoryIDtoDelete, reassignToCategoryID)` metodo. Questo metodo è necessario creare un'istanza del `CategoriesTableAdapter` e `ProductsTableAdapter` e quindi impostare questi due oggetti TableAdapter `Connection` proprietà allo stesso `SqlConnection` istanza. In questo punto, uno degli oggetti due TableAdapter avvia la transazione con una chiamata a `BeginTransaction`. Vengano richiamati i metodi TableAdapter per riassegnare i prodotti e la categoria viene eliminata in un `Try...Catch` blocco con la transazione di commit o rollback nuovamente in base alle esigenze.

## <a name="step-4-adding-theupdatewithtransactionmethod-to-the-business-logic-layer"></a>Passaggio 4: Aggiunta di`UpdateWithTransaction`al livello di logica di Business (metodo)

Nel passaggio 3 è stato aggiunto un `UpdateWithTransaction` metodo il `ProductsTableAdapter` in DAL. È necessario aggiungere un metodo corrispondente al livello Business Logic. Anche se il livello di presentazione può chiamare direttamente down per richiamare il `UpdateWithTransaction` (metodo), queste esercitazioni sono strived definire un'architettura a più livelli che isola dal livello di presentazione. Pertanto, behooves di questo approccio di continuare.

Aprire il `ProductsBLL` file di classe e aggiungere un metodo denominato `UpdateWithTransaction` che chiami fino al metodo DAL corrispondente. Sono due nuovi metodi in questo punto dovrebbe essere `ProductsBLL`: `UpdateWithTransaction`, che appena aggiunto, e `DeleteProductsWithTransaction`, cui è stato aggiunto nel passaggio 3.


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample6.vb)]

> [!NOTE]
> Questi metodi non includono il `DataObjectMethodAttribute` attributo assegnato per la maggior parte degli altri metodi di `ProductsBLL` classe perché ci sarà richiamare tali metodi direttamente dalle classi di codice di pagine ASP.NET. Tenere presente che `DataObjectMethodAttribute` consente di contrassegnare i metodi devono essere visualizzato in s ObjectDataSource origine dati di configurazione guidata e in quale scheda (SELECT, UPDATE, INSERT o DELETE). Poiché il controllo GridView non dispone di qualsiasi supporto incorporato per la modifica o eliminazione in batch, è necessario richiamare tali metodi a livello di programmazione anziché utilizzare l'approccio senza codice dichiarativo.


## <a name="step-5-atomically-updating-database-data-from-the-presentation-layer"></a>Passaggio 5: Aggiornamento atomico dei dati di Database dal livello di presentazione

Per illustrare l'effetto della transazione durante l'aggiornamento di un batch di record, s consentono di creare un'interfaccia utente che elenca tutti i prodotti in un controllo GridView e include un pulsante di controllo che, quando si fa clic, riassegna i prodotti `CategoryID` valori. In particolare, la riassegnazione di categoria avanzamento in modo che i primi prodotti diversi vengono assegnati un valido `CategoryID` valore mentre altri sono intenzionalmente assegnato un inesistente `CategoryID` valore. Se si tenta di aggiornare il database con un prodotto il cui `CategoryID` non corrisponde a una categoria esistente s `CategoryID`, si verificherà una violazione di vincolo di chiave esterna e verrà generata un'eccezione. Si noterà in questo esempio è che quando si utilizza una transazione, l'eccezione generata dalla violazione di vincolo di chiave esterna causerà precedente valido `CategoryID` modifiche di eseguire il rollback. Quando non si utilizza una transazione, tuttavia, rimangono le modifiche alle categorie iniziale.

Aprire il `Transactions.aspx` nella pagina di `BatchData` cartelle e trascinare un controllo GridView dalla casella degli strumenti nella finestra di progettazione. Impostare il relativo `ID` a `Products` e dal suo smart tag, associarlo a un nuovo oggetto ObjectDataSource denominato `ProductsDataSource`. Configurare ObjectDataSource per estrarre i dati di `ProductsBLL` classe s `GetProducts` metodo. Questo verrà un GridView di sola lettura, pertanto, impostare gli elenchi a discesa nell'aggiornamento, inserimento ed eliminare le tabulazioni su (nessuno) e fare clic su Fine.


[![Configurare ObjectDataSource per utilizzare il metodo GetProducts ProductsBLL classe s](wrapping-database-modifications-within-a-transaction-vb/_static/image5.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image3.png)

**Figura 5**: configurare ObjectDataSource per utilizzare il `ProductsBLL` classe s `GetProducts` metodo ([fare clic per visualizzare l'immagine ingrandita](wrapping-database-modifications-within-a-transaction-vb/_static/image4.png))


[![Impostare gli elenchi a discesa in UPDATE, INSERT ed eliminare le tabulazioni su (nessuno)](wrapping-database-modifications-within-a-transaction-vb/_static/image6.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image5.png)

**Figura 6**: impostare l'elenco a discesa sono elencati nelle schede DELETE, INSERT e UPDATE (nessuno) ([fare clic per visualizzare l'immagine ingrandita](wrapping-database-modifications-within-a-transaction-vb/_static/image6.png))


Dopo aver completato la configurazione guidata origine dati, Visual Studio creerà BoundField e un CheckBoxField per i campi di dati di prodotto. Rimuovere tutti questi campi, ad eccezione di `ProductID`, `ProductName`, `CategoryID`, e `CategoryName` e rinominare il `ProductName` e `CategoryName` BoundField `HeaderText` proprietà Product e Category, rispettivamente. Smart tag, selezionare l'opzione Abilita Paging. Dopo aver apportato queste modifiche, il markup dichiarativo s GridView e ObjectDataSource dovrebbe essere simile al seguente:


[!code-aspx[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample7.aspx)]

Successivamente, aggiungere tre controlli Web pulsante sopra il controllo GridView. Impostare il primo pulsante s proprietà Text su Aggiorna la griglia, il secondo s per modificare le categorie (con transazione) e il terzo una s per modificare le categorie (senza transazione).


[!code-aspx[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample8.aspx)]

La visualizzazione di progettazione in Visual Studio a questo punto dovrebbe essere simile alla schermata illustrata nella figura 7.


[![La pagina contiene un oggetto GridView con tre pulsanti Web](wrapping-database-modifications-within-a-transaction-vb/_static/image7.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image7.png)

**Figura 7**: la pagina contiene un oggetto GridView con tre pulsanti Web ([fare clic per visualizzare l'immagine ingrandita](wrapping-database-modifications-within-a-transaction-vb/_static/image8.png))


Creare gestori eventi per ogni pulsante tre s `Click` eventi e utilizzare il codice seguente:


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample9.vb)]

Aggiornamento pulsante s `Click` gestore semplicemente nuovamente l'associazione dati a GridView mediante la chiamata di `Products` GridView s `DataBind` metodo.

Il secondo gestore dell'evento Riassegna i prodotti `CategoryID` s e viene utilizzato il nuovo metodo di transazione da BLL per eseguire il database Aggiorna sotto ombrello di una transazione. Si noti che ogni prodotto s `CategoryID` arbitrariamente è impostato sullo stesso valore come relativo `ProductID`. Questa tecnica funziona correttamente per il primo alcuni prodotti, poiché tali prodotti siano `ProductID` valori che si verificano per eseguire il mapping a valido `CategoryID` s. Ma una volta il `ProductID` inizio s diventi troppo grande, questa sovrapposizione casuale di `ProductID` s e `CategoryID` s non è più valido.

Il terzo `Click` gestore degli aggiornamenti dei prodotti `CategoryID` s nello stesso modo, ma invia l'aggiornamento al database tramite il `ProductsTableAdapter` predefinito s `Update` metodo. Questo `Update` metodo non va a capo la serie di comandi all'interno di una transazione, pertanto le modifiche vengono applicate prima il primo errore di violazione di vincolo di chiave esterna è stato rilevato verranno mantenuti.

Per illustrare questo comportamento, visitare la pagina tramite un browser. Inizialmente verrà visualizzato la prima pagina di dati come illustrato nella figura 8. Successivamente, fare clic sul pulsante di modificare le categorie (con transazione). Ciò causerà un postback e tentare di aggiornare tutti i prodotti `CategoryID` valori, ma comporta una violazione di vincolo di chiave esterna (vedere Figura 9).


[![I prodotti vengono visualizzati in un controllo GridView paginabile](wrapping-database-modifications-within-a-transaction-vb/_static/image8.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image9.png)

**Figura 8**: i prodotti di vengono visualizzati in GridView paginabile ([fare clic per visualizzare l'immagine ingrandita](wrapping-database-modifications-within-a-transaction-vb/_static/image10.png))


[![Riassegnazione di categorie comporta una violazione di vincolo di chiave esterna](wrapping-database-modifications-within-a-transaction-vb/_static/image9.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image11.png)

**Figura 9**: riassegnare i risultati di categorie in una violazione di vincolo di chiave esterna ([fare clic per visualizzare l'immagine ingrandita](wrapping-database-modifications-within-a-transaction-vb/_static/image12.png))


A questo punto premere il pulsante Indietro del browser s e quindi fare clic sul pulsante Aggiorna la griglia. Dopo l'aggiornamento dei dati verrà visualizzato lo stesso output esattamente come illustrato nella figura 8. Vale a dire, anche se alcuni prodotti `CategoryID` s sono valori modificati per legali e aggiornata correttamente nel database, sono state annullate quando si è verificata la violazione di vincolo di chiave esterna.

Provare a fare clic sul pulsante Modifica categorie (senza transazione). Verrà creato lo stesso errore di violazione di vincolo di chiave esterna (vedere Figura 9), ma questa volta, i prodotti il cui `CategoryID` i valori sono stati modificati a giuridica valore non sarà possibile eseguire il rollback. Premere il pulsante Indietro del browser s e quindi sul pulsante Aggiorna la griglia. Come mostrato nella figura 10, il `CategoryID` s dei primi otto prodotti sono stati riassegnati. Nella figura 8, ad esempio, registrazione aveva un `CategoryID` pari a 1, ma nella figura 10 it s è stato riassegnato a 2.


[![Alcuni valori CategoryID prodotti sono stati aggiornati mentre altri sono stati non](wrapping-database-modifications-within-a-transaction-vb/_static/image10.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image13.png)

**Figura 10**: alcuni prodotti `CategoryID` valori aggiornati mentre altri sono stati non erano ([fare clic per visualizzare l'immagine ingrandita](wrapping-database-modifications-within-a-transaction-vb/_static/image14.png))


## <a name="summary"></a>Riepilogo

Per impostazione predefinita, i metodi s TableAdapter non racchiudere le istruzioni di database eseguito all'interno dell'ambito di una transazione, ma con qualche operazione è possibile aggiungere i metodi che consente di creare, commit e rollback di una transazione. In questa esercitazione è stata creata in tre metodi di `ProductsTableAdapter` classe: `BeginTransaction`, `CommitTransaction`, e `RollbackTransaction`. È stato illustrato come utilizzare questi metodi insieme a un `Try...Catch` blocco di rendere atomico una serie di istruzioni di modifica dei dati. In particolare, è stato creato il `UpdateWithTransaction` metodo il `ProductsTableAdapter`, che usa il modello di aggiornamento Batch per eseguire le modifiche necessarie alle righe di una classe fornita `ProductsDataTable`. Abbiamo anche aggiunto il `DeleteProductsWithTransaction` metodo il `ProductsBLL` classe BLL, che accetta un `List` di `ProductID` valori come input e chiama il metodo di modello di DB-Direct `Delete` per ogni `ProductID`. Entrambi i metodi per iniziare, creazione di una transazione e quindi l'esecuzione di istruzioni di modifica dei dati all'interno di un `Try...Catch` blocco. Se si verifica un'eccezione, viene eseguito il rollback della transazione, in caso contrario viene eseguito il commit.

Passaggio 5 è stato illustrato l'effetto degli aggiornamenti di batch transazionale e aggiornamenti batch che trascurati per utilizzare una transazione. Nelle esercitazioni successive tre verrà si basano su base illustrata in questa esercitazione e creare interfacce utente per l'esecuzione di inserimenti, eliminazioni e aggiornamenti in batch.

Buona programmazione!

## <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Mantenere la coerenza del Database con transazioni](http://aspnet.4guysfromrolla.com/articles/072705-1.aspx)
- [Stored procedure per la gestione delle transazioni in SQL Server](http://www.4guysfromrolla.com/webtech/080305-1.shtml)
- [Transazioni da rendere più semplice: `System.Transactions`](https://blogs.msdn.com/florinlazar/archive/2004/07/23/192239.aspx)
- [TransactionScope e DataAdapter](http://andyclymer.blogspot.com/2007/01/transactionscope-and-dataadapters.html)
- [Utilizzo delle transazioni di Database Oracle in .NET](http://www.oracle.com/technology/pub/articles/price_dbtrans_dotnet.html)

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Lead revisori per questa esercitazione sono stati Dave Gardner Hilton Giesenow e Teresa Murphy. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](batch-inserting-cs.md)
> [Successivo](batch-updating-vb.md)
