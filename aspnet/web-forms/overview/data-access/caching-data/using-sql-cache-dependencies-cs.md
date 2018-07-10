---
uid: web-forms/overview/data-access/caching-data/using-sql-cache-dependencies-cs
title: Uso delle dipendenze della Cache SQL (c#) | Microsoft Docs
author: rick-anderson
description: La strategia di memorizzazione nella cache più semplice consiste nel consentire dei dati memorizzati nella cache scadono dopo un periodo di tempo specificato. Ma questo semplice approccio significa che i dati memorizzati nella cache maintai...
ms.author: aspnetcontent
ms.date: 05/30/2007
ms.assetid: 0e91842c-7f10-4aed-8c23-4ee3e2774014
msc.legacyurl: /web-forms/overview/data-access/caching-data/using-sql-cache-dependencies-cs
msc.type: authoredcontent
ms.openlocfilehash: 3401babaa3541bcf214f778b5ad4f1ea4ae76b38
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802586"
---
<a name="using-sql-cache-dependencies-c"></a>Uso delle dipendenze della Cache SQL (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare il codice](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_61_CS.zip) o [Scarica il PDF](using-sql-cache-dependencies-cs/_static/datatutorial61cs1.pdf)

> La strategia di memorizzazione nella cache più semplice consiste nel consentire dei dati memorizzati nella cache scadono dopo un periodo di tempo specificato. Ma questo semplice approccio significa che i dati memorizzati nella cache non gestisce alcuna associazione con l'origine dati sottostante, generando dati non aggiornati che viene mantenuti troppo lungo o i dati correnti che scade troppo presto. Un approccio migliore consiste nell'utilizzare la classe SqlCacheDependency in modo che i dati rimangono memorizzati nella cache fino a quando i dati di sottostanti sono stati modificati nel database SQL. Questa esercitazione spiega come fare.


## <a name="introduction"></a>Introduzione

Esaminate le tecniche di memorizzazione nella cache le [dati la memorizzazione nella cache con ObjectDataSource](caching-data-with-the-objectdatasource-cs.md) e [dati la memorizzazione nella cache nell'architettura](caching-data-in-the-architecture-cs.md) esercitazioni consentono di rimuovere i dati dalla cache dopo aver specificato una scadenza basati sul tempo periodo. Questo approccio è il modo più semplice per bilanciare il miglioramento delle prestazioni della memorizzazione nella cache contro l'obsolescenza dei dati. Selezionando una scadenza ora di *x* secondi, lo sviluppatore della pagina concedes per sfruttare i vantaggi della memorizzazione nella cache solo per le prestazioni *x* in secondi, ma possono dormire che suoi dati non sarà mai aggiornati più di un massimo dei *x* secondi. Naturalmente, per i dati statici, *x* può essere esteso al ciclo di vita dell'applicazione web, come è stato esaminato nel [memorizzazione dei dati all'avvio dell'applicazione](caching-data-at-application-startup-cs.md) esercitazione.

Quando la memorizzazione nella cache i dati del database, una scadenza basati sul tempo è spesso scelti per la facilità d'uso, ma è spesso una soluzione insufficiente. In teoria, i dati del database rimangono memorizzati nella cache fino a quando i dati sottostanti sono stati modificati nel database. solo a questo punto potrebbe essere eliminata la cache. Questo approccio ottimizza le prestazioni della memorizzazione nella cache e riduce al minimo la durata dei dati non aggiornati. Tuttavia, per sfruttare questi vantaggi sono deve essere un sistema che sa quando i dati del database sottostante sono stati modificati e rimuove gli elementi corrispondenti dalla cache. Prima di ASP.NET 2.0, gli sviluppatori di pagine sono stati responsabili dell'implementazione di questo sistema.

ASP.NET 2.0 fornisce un' [ `SqlCacheDependency` classe](https://msdn.microsoft.com/library/system.web.caching.sqlcachedependency.aspx) e l'infrastruttura necessaria per determinare quando è stata modificata nel database in modo che i corrispondenti elementi memorizzati nella cache può essere eliminata. Esistono due tecniche per determinare quando vengono modificati i dati sottostanti: notifica e il polling. Dopo aver illustrato le differenze tra la notifica e polling, si creerà l'infrastruttura necessaria per supportare il polling e quindi illustrato come usare il `SqlCacheDependency` scenari classe nella sintassi dichiarativa e a livello di codice.

## <a name="understanding-notification-and-polling"></a>Polling e informazioni sulle notifiche

Esistono due tecniche che possono essere utilizzate per determinare quando i dati in un database sono stati modificati: la notifica e polling. Con la notifica, il database runtime di ASP.NET viene avvisato automaticamente quando i risultati di una particolare query sono stati modificati dopo l'ultima esecuzione della query, a questo punto vengono rimossi gli elementi memorizzati associati alla query. Con il polling, il server di database mantiene le informazioni su quando particolari tabelle ultimo sono state aggiornate. Il runtime ASP.NET esegue periodicamente il polling del database per verificare cosa di cui le tabelle siano state modificate perché sono stati immessi nella cache. Tali tabelle i cui dati sono stati modificati disporre gli elementi della cache associata rimossi.

L'opzione di notifica richiede meno il programma di installazione di polling ed è più granulare poiché consente di tenere traccia delle modifiche a livello di query anziché a livello di tabella. Sfortunatamente, le notifiche sono disponibili solo nelle edizioni complete di Microsoft SQL Server 2005 (ad esempio, le edizioni non Express). Tuttavia, l'opzione di polling è utilizzabile per tutte le versioni di Microsoft SQL Server da 7.0 a 2005. Poiché queste esercitazioni usano l'edizione Express di SQL Server 2005, ci concentreremo sulla configurazione e utilizzo dell'opzione di polling. Alla fine di questa esercitazione per ulteriori risorse sulle funzionalità di notifica di SQL Server 2005 s, consultare la sezione ulteriormente la lettura.

Con il polling, il database deve essere configurato in modo da includere una tabella denominata `AspNet_SqlCacheTablesForChangeNotification` con tre colonne: `tableName`, `notificationCreated`, e `changeId`. Questa tabella contiene una riga per ogni tabella che contiene i dati che potrebbero dover essere utilizzato in una dipendenza della cache SQL nell'applicazione web. Il `tableName` colonna specifica il nome della tabella durante `notificationCreated` indica la data e ora la riga è stata aggiunta alla tabella. Il `changeId` sloupec JE typu `int` e ha un valore iniziale pari a 0. Il valore viene incrementato con ogni modifica alla tabella.

Oltre al `AspNet_SqlCacheTablesForChangeNotification` tabella, il database deve inoltre includere i trigger su ognuna delle tabelle che possono essere visualizzati in una dipendenza della cache SQL. Questi trigger vengono eseguiti ogni volta che una riga inserita, aggiornata o eliminata e incrementare la tabella s `changeId` valore `AspNet_SqlCacheTablesForChangeNotification`.

Il runtime ASP.NET tiene traccia corrente `changeId` per una tabella quando la memorizzazione nella cache i dati utilizzando un `SqlCacheDependency` oggetto. Periodicamente viene controllato il database e l'eventuale `SqlCacheDependency` oggetti la cui proprietà `changeId` è diverso da quello del database vengono eliminate dopo che un differisce `changeId` valore indica che si è verificato una modifica alla tabella perché è stato memorizzato nella cache i dati.

## <a name="step-1-exploring-theaspnetregsqlexecommand-line-program"></a>Passaggio 1: Esplorazione di`aspnet_regsql.exe`programma della riga di comando

Con l'approccio di polling il database deve essere configurato per contenere l'infrastruttura descritto in precedenza: una tabella predefinita (`AspNet_SqlCacheTablesForChangeNotification`), un numero limitato di stored procedure e trigger in ognuna delle tabelle che possono essere utilizzate nelle dipendenze della cache SQL nel web applicazione. Queste tabelle, stored procedure e trigger possono essere creati tramite il programma della riga di comando `aspnet_regsql.exe`, che si trova nel `$WINDOWS$\Microsoft.NET\Framework\version` cartella. Per creare il `AspNet_SqlCacheTablesForChangeNotification` tabella e stored procedure associate, eseguire il comando seguente dalla riga di comando:


[!code-console[Main](using-sql-cache-dependencies-cs/samples/sample1.cmd)]

> [!NOTE]
> Per eseguire questi comandi deve essere l'account di accesso di database specificato il [ `db_securityadmin` ](https://msdn.microsoft.com/library/ms188685.aspx) e [ `db_ddladmin` ](https://msdn.microsoft.com/library/ms190667.aspx) ruoli. Per esaminare il T-SQL inviate al database per il `aspnet_regsql.exe` programma della riga di comando, vedere [questo post di blog](http://scottonwriting.net/sowblog/posts/10709.aspx).


Ad esempio, per aggiungere l'infrastruttura per il polling di un database Microsoft SQL Server denominate `pubs` in un server di database denominato `ScottsServer` usando l'autenticazione di Windows, passare alla directory appropriata e, dalla riga di comando, immettere:


[!code-console[Main](using-sql-cache-dependencies-cs/samples/sample2.cmd)]

Dopo aver aggiunto l'infrastruttura a livello di database, è necessario aggiungere trigger alle tabelle che verranno usate nelle dipendenze della cache SQL. Usare il `aspnet_regsql.exe` riga di comando del programma nuovamente, ma specificare il nome di tabella usando il `-t` passare e invece di usare il `-ed` switch Usa `-et`, come illustrato di seguito:


[!code-html[Main](using-sql-cache-dependencies-cs/samples/sample3.html)]

Per aggiungere i trigger per il `authors` e `titles` tabelle sulle `pubs` il database in `ScottsServer`, usare:


[!code-console[Main](using-sql-cache-dependencies-cs/samples/sample4.cmd)]

Per questa esercitazione, aggiungere il trigger per il `Products`, `Categories`, e `Suppliers` tabelle. Esamineremo la sintassi della riga di comando specifico nel passaggio 3.

## <a name="step-2-referencing-a-microsoft-sql-server-2005-express-edition-database-inappdata"></a>Passaggio 2: Fa riferimento a un Database Microsoft SQL Server 2005 Express Edition in`App_Data`

Il `aspnet_regsql.exe` programma della riga di comando richiede il nome del database e server per aggiungere l'infrastruttura di polling necessarie. Ma qual è il nome del database e server per un database Microsoft SQL Server 2005 Express che si trova nel `App_Data` cartella? Invece di dover individuare quali sono i nomi di database e server, ho ve ha rilevato che l'approccio più semplice per collegare il database per il `localhost\SQLExpress` istanza del database e rinominare i dati usando [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx). Se si dispone di una delle versioni complete di SQL Server 2005 installato nel computer, quindi probabilmente già installato nel computer SQL Server Management Studio. Se si dispone solo di Developer Express edition, è possibile scaricare la versione gratuita [Microsoft SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?displaylang=en&amp;FamilyID=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796).

Per iniziare, chiudere Visual Studio. Successivamente, aprire SQL Server Management Studio e scegliere di connettersi al `localhost\SQLExpress` server utilizzando l'autenticazione di Windows.


![Collegare a localhost\SQLExpress Server](using-sql-cache-dependencies-cs/_static/image1.gif)

**Figura 1**: per collegare il `localhost\SQLExpress` Server


Dopo la connessione al server Management Studio verranno Mostra i server e avere sottocartelle per il database, sicurezza e così via. Pulsante destro del mouse sulla cartella database e scegliere l'opzione di connessione. Verrà visualizzata la finestra di dialogo Collega database (vedere la figura 2). Fare clic sul pulsante Aggiungi e selezionare il `NORTHWND.MDF` cartella del database nella raccolta di s dell'applicazione web `App_Data` cartella.


[![Collegare il NORTHWND. Database MDF dalla cartella App_Data](using-sql-cache-dependencies-cs/_static/image2.gif)](using-sql-cache-dependencies-cs/_static/image1.png)

**Figura 2**: collegare il `NORTHWND.MDF` del Database delle `App_Data` cartella ([fare clic per visualizzare l'immagine con dimensioni normali](using-sql-cache-dependencies-cs/_static/image2.png))


Si aggiungerà il database per la cartella del database. Il nome del database potrebbe essere il percorso completo al file di database o il percorso completo è preceduto da un [GUID](http://en.wikipedia.org/wiki/Globally_Unique_Identifier). Per evitare di dover digitare il nome del database lungo quando si usa aspnet\_regsql.exe strumento da riga di comando, rinominare il database a un nome più Converto facendo clic sul database appena collegato e scegliendo Rinomina. Ho ve rinominata DataTutorials mio database.


![Rinominare il Database collegato con un nome più Converto](using-sql-cache-dependencies-cs/_static/image3.gif)

**Figura 3**: rinominare il Database collegato con un nome più Converto


## <a name="step-3-adding-the-polling-infrastructure-to-the-northwind-database"></a>Passaggio 3: Aggiunta l'infrastruttura di Polling per il Database Northwind

Ora che è stata associata la `NORTHWND.MDF` del database del `App_Data` cartella, sono pronti per aggiungere l'infrastruttura di polling. Supponendo che il database è già rinominato a DataTutorials, eseguire i seguenti quattro comandi:


[!code-console[Main](using-sql-cache-dependencies-cs/samples/sample5.cmd)]

Dopo aver eseguito questi quattro comandi, fare doppio clic sul nome del database in Management Studio, passare il sottomenu di attività e scegliere Disconnetti. Quindi chiudere Management Studio e riaprire Visual Studio.

Una volta è riaperto Visual Studio, esaminare il database tramite Esplora Server. Si noti la nuova tabella (`AspNet_SqlCacheTablesForChangeNotification`), la nuova stored procedure e trigger nel `Products`, `Categories`, e `Suppliers` tabelle.


![Il Database include ora l'infrastruttura necessaria Polling](using-sql-cache-dependencies-cs/_static/image4.gif)

**Figura 4**: il Database include ora l'infrastruttura necessaria Polling


## <a name="step-4-configuring-the-polling-service"></a>Passaggio 4: Configurazione del servizio di Polling

Dopo aver creato le tabelle necessarie, i trigger e stored procedure nel database, il passaggio finale consiste nel configurare il servizio di polling, che viene eseguito tramite `Web.config` specificando i database da usare e la frequenza di polling espresso in millisecondi. Il markup seguente esegue il polling del database Northwind, una volta al secondo.


[!code-xml[Main](using-sql-cache-dependencies-cs/samples/sample6.xml)]

Il `name` valore di `<add>` elemento (NorthwindDB) associa un nome leggibile dall'utente a un determinato database. Quando si lavora con le dipendenze della cache SQL, è necessario fare riferimento al nome del database definito in questa sezione, nonché la tabella basata su dati memorizzati nella cache. Si vedrà come usare il `SqlCacheDependency` classe da associare a livello di programmazione delle dipendenze della cache SQL con memorizzato nella cache i dati nel passaggio 6.

Dopo aver stabilita una dipendenza della cache SQL, il sistema di polling si connetterà ai database di cui è definiti nel `<databases>` elementi ogni `pollTime` millisecondi ed eseguire il `AspNet_SqlCachePollingStoredProcedure` stored procedure. Questa stored procedure - è stato aggiunto di nuovo il passaggio 3 utilizzando il `aspnet_regsql.exe` strumento da riga di comando - restituisce la `tableName` e `changeId` i valori per ciascun record in `AspNet_SqlCacheTablesForChangeNotification`. Obsolete le dipendenze della cache SQL vengono rimossi dalla cache.

Il `pollTime` impostazione introduce un compromesso tra prestazioni e obsolescenza dei dati. Un piccolo `pollTime` valore aumenta il numero di richieste al database, ma più rapidamente rimuove i dati non aggiornati dalla cache. Una maggiore `pollTime` valore riduce il numero di richieste al database, ma aumenta il ritardo tra quando i dati di back-end viene modificato e quando vengono eliminati gli elementi correlati. Fortunatamente, la richiesta del database è in esecuzione una semplice stored procedure che s restituzione solo poche righe di una tabella semplice e leggera. Ma sperimentare diverse `pollTime` decadimento ristretto di accesso e i dati per l'applicazione di valori per trovare un compromesso ideale tra database. Il più piccolo `pollTime` valore consentito è 500.

> [!NOTE]
> Nell'esempio sopra riportato fornisce un'unica `pollTime` valore il `<sqlCacheDependency>` elemento, ma è possibile specificare facoltativamente il `pollTime` valore nel `<add>` elemento. Ciò è utile se si dispone di più database specificati e si desidera personalizzare la frequenza di polling per ogni database.


## <a name="step-5-declaratively-working-with-sql-cache-dependencies"></a>Passaggio 5: Funziona in modo dichiarativo con dipendenze della Cache SQL

In passaggi da 1 a 4 è stato illustrato come configurare l'infrastruttura di database necessari e configurare il sistema di polling. Con questa infrastruttura, è ora possibile aggiungere elementi alla cache dei dati con una dipendenza della cache SQL associata utilizzando tecniche a livello di codice o dichiarative. In questo passaggio verrà esaminato come lavorare in modo dichiarativo con dipendenze della cache SQL. Nel passaggio 6 esamineremo l'approccio programmatico.

Il [dati la memorizzazione nella cache con ObjectDataSource](caching-data-with-the-objectdatasource-cs.md) esercitazione esplorato le funzionalità di memorizzazione nella cache dichiarative di ObjectDataSource. Impostando semplicemente il `EnableCaching` proprietà `true` e il `CacheDuration` proprietà a un intervallo di tempo, ObjectDataSource inserirà automaticamente nella cache i dati restituiti dal relativo oggetto sottostante per l'intervallo specificato. ObjectDataSource è anche possibile usare uno o più dipendenze della cache SQL.

Per illustrare l'utilizzo delle dipendenze della cache SQL in modo dichiarativo, aprire il `SqlCacheDependencies.aspx` nella pagina di `Caching` cartelle e trascinare un controllo GridView dalla casella degli strumenti nella finestra di progettazione. Impostare la s GridView `ID` al `ProductsDeclarative` e scegliere dal suo smart tag da associare a un nuovo oggetto ObjectDataSource denominato `ProductsDataSourceDeclarative`.


[![Creare un nuovo oggetto ObjectDataSource denominato ProductsDataSourceDeclarative](using-sql-cache-dependencies-cs/_static/image5.gif)](using-sql-cache-dependencies-cs/_static/image3.png)

**Figura 5**: creare un nuovo oggetto ObjectDataSource denominato `ProductsDataSourceDeclarative` ([fare clic per visualizzare l'immagine con dimensioni normali](using-sql-cache-dependencies-cs/_static/image4.png))


Configurare ObjectDataSource per usare la `ProductsBLL` classe e impostare l'elenco a discesa nella scheda Seleziona `GetProducts()`. Nella scheda aggiornamento, scegliere il `UpdateProduct` rapporto di overload con tre parametri di input - `productName`, `unitPrice`, e `productID`. Impostare gli elenchi di riepilogo a discesa su (nessuno) nelle schede INSERT e DELETE.


[![Usare l'Overload UpdateProduct con tre parametri di Input](using-sql-cache-dependencies-cs/_static/image6.gif)](using-sql-cache-dependencies-cs/_static/image5.png)

**Figura 6**: usare l'Overload UpdateProduct con tre parametri di Input ([fare clic per visualizzare l'immagine con dimensioni normali](using-sql-cache-dependencies-cs/_static/image6.png))


[![Impostazione dell'elenco di riepilogo a discesa su (nessuno) per le schede DELETE e INSERT](using-sql-cache-dependencies-cs/_static/image7.gif)](using-sql-cache-dependencies-cs/_static/image7.png)

**Figura 7**: impostare l'elenco a discesa su (nessuno) per l'inserimento ed eliminare schede ([fare clic per visualizzare l'immagine con dimensioni normali](using-sql-cache-dependencies-cs/_static/image8.png))


Dopo aver completato la procedura guidata Configura origine dati, Visual Studio creerà CheckBoxFields e BoundField in GridView per ognuno dei campi dati. Rimuovere tutti i campi, ma `ProductName`, `CategoryName`, e `UnitPrice`e formattare i campi nel modo desiderato. GridView s nello smart tag, selezionare le caselle di controllo Abilita Paging, abilitare l'ordinamento e Abilita modifica. Visual Studio imposterà la s ObjectDataSource `OldValuesParameterFormatString` proprietà `original_{0}`. Affinché la funzionalità di modifica GridView s per il corretto funzionamento, rimuovere questa proprietà interamente dalla sintassi dichiarativa o impostarlo nuovamente sul valore predefinito, `{0}`.

Infine, aggiungere un controllo etichetta Web sopra il controllo GridView e il set relativo `ID` proprietà `ODSEvents` e il relativo `EnableViewState` proprietà `false`. Dopo aver apportato queste modifiche, il markup dichiarativo s pagina dovrebbe essere simile al seguente. Si noti che se ve apportate numerose personalizzazioni estetiche ai campi GridView che non sono necessari per dimostrare la funzionalità di dipendenza della cache SQL.


[!code-aspx[Main](using-sql-cache-dependencies-cs/samples/sample7.aspx)]

Successivamente, creare un gestore eventi per gli oggetti ObjectDataSource `Selecting` eventi e nell'aggiungere il codice seguente:


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample8.cs)]

Si tenga presente che gli oggetti ObjectDataSource `Selecting` evento viene generato solo quando si recuperano dati dal relativo oggetto sottostante. Se l'oggetto ObjectDataSource accede ai dati dalla cache, non viene generato questo evento.

A questo punto, visitare questa pagina tramite un browser. Poiché è fornire un supporto iniziale per implementare qualsiasi la memorizzazione nella cache, ogni volta che si pagina, ordinamento o modifica la pagina della griglia deve visualizzare il testo, se si seleziona evento generato, come illustrato nella figura 8.


[![ObjectDataSource s quando si seleziona evento viene generato ogni volta che il controllo GridView viene eseguito il paging, modificato, o Sorted](using-sql-cache-dependencies-cs/_static/image8.gif)](using-sql-cache-dependencies-cs/_static/image9.png)

**Figura 8**: s The ObjectDataSource `Selecting` evento viene attivato ogni ora viene eseguito il paging GridView, modificata o Sorted ([fare clic per visualizzare l'immagine con dimensioni normali](using-sql-cache-dependencies-cs/_static/image10.png))


Come abbiamo visto nel [dati la memorizzazione nella cache con ObjectDataSource](caching-data-with-the-objectdatasource-cs.md) esercitazione, l'impostazione il `EnableCaching` proprietà `true` fa sì che ObjectDataSource memorizzare nella cache i dati per la durata specificata dalla relativa `CacheDuration` proprietà. ObjectDataSource ha anche un [ `SqlCacheDependency` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.sqlcachedependency.aspx), che aggiunge uno o più dipendenze della cache SQL ai dati memorizzati nella cache usando il modello:


[!code-css[Main](using-sql-cache-dependencies-cs/samples/sample9.css)]

In cui *databaseName* è il nome del database come specificato nella `name` attributo del `<add>` elemento `Web.config`, e *tableName* è il nome della tabella di database. Ad esempio, per creare un oggetto ObjectDataSource che memorizza nella cache i dati per un periodo illimitato basata su una dipendenza della cache SQL con gli oggetti di Northwind `Products` tabella, impostare la s ObjectDataSource `EnableCaching` proprietà `true` e il relativo `SqlCacheDependency` proprietà NorthwindDB:Products.

> [!NOTE]
> È possibile usare una dipendenza della cache SQL *e* una scadenza basati sul tempo impostando `EnableCaching` al `true`, `CacheDuration` per l'intervallo di tempo e `SqlCacheDependency` per i nomi dei database e tabella. ObjectDataSource rimuoverà i dati quando viene raggiunta la scadenza basati sul tempo o quando il sistema di polling viene rilevato che i dati del database sottostante è stato modificato, a seconda di quale si verifica per primo.


Il controllo GridView in `SqlCacheDependencies.aspx` sono riportati i dati da due tabelle - `Products` e `Categories` (il prodotto s `CategoryName` campo viene recuperato tramite un `JOIN` su `Categories`). Pertanto, si vuole specificare due dipendenze della cache SQL: NorthwindDB:Products; NorthwindDB:Categories.


[![Configurare ObjectDataSource per supportare la memorizzazione nella cache usando le dipendenze della Cache SQL sui prodotti e le categorie](using-sql-cache-dependencies-cs/_static/image9.gif)](using-sql-cache-dependencies-cs/_static/image11.png)

**Figura 9**: configurare ObjectDataSource per il supporto di memorizzazione nella cache usando SQL delle dipendenze della Cache nel `Products` e `Categories` ([fare clic per visualizzare l'immagine con dimensioni normali](using-sql-cache-dependencies-cs/_static/image12.png))


Dopo aver configurato ObjectDataSource per supportare la memorizzazione nella cache, visitare di nuovo la pagina tramite un browser. Anche in questo caso, l'evento di selezione di testo generato deve essere visualizzato nella prima visita pagina, ma non verrà più visualizzato quando il paging, l'ordinamento o fare clic sul pulsante Modifica oppure su Annulla. Infatti, dopo i dati vengono caricati nella cache s ObjectDataSource, rimane presente finché il `Products` o `Categories` tabelle vengono modificate o i dati vengono aggiornati tramite il controllo GridView.

Dopo aver attivato il paging attraverso la griglia e notare l'assenza dell'evento quando si seleziona il testo, aprire una nuova finestra del browser e passare all'Esercitazione nozioni di base di modifica, inserimento ed eliminazione di sezione (`~/EditInsertDelete/Basics.aspx`). Aggiornare il nome o il prezzo di un prodotto. Quindi, da visualizzare una pagina diversa dei dati per la prima finestra del browser, ordinare la griglia o fare clic su un pulsante di modifica di riga o le righe. Questa fase, di generazione dell'evento di selezione deve visualizzare nuovamente, come il database sottostante i dati sono stati modificati (vedere la figura 10). Se il testo non viene visualizzato, attendere alcuni istanti e ripetere l'operazione. Tenere presente che il servizio di polling è controllo delle modifiche per il `Products` tabella ogni `pollTime` millisecondi, in modo che si verifica un ritardo tra quando i dati sottostanti vengono aggiornati e quando i dati memorizzati nella cache vengano rimosso.


[![Modifica della tabella Products rimuove i dati memorizzati nella cache del prodotto](using-sql-cache-dependencies-cs/_static/image10.gif)](using-sql-cache-dependencies-cs/_static/image13.png)

**Figura 10**: modifica della tabella Products rimuove i dati del prodotto memorizzati nella cache ([fare clic per visualizzare l'immagine con dimensioni normali](using-sql-cache-dependencies-cs/_static/image14.png))


## <a name="step-6-programmatically-working-with-thesqlcachedependencyclass"></a>Passaggio 6: Uso a livello di codice con il`SqlCacheDependency`classe

Il [dati la memorizzazione nella cache nell'architettura](caching-data-in-the-architecture-cs.md) esercitazione preso in esame i vantaggi dell'uso di un altro livello di memorizzazione nella cache nell'architettura invece che consente di connettere la memorizzazione nella cache con ObjectDataSource. In questa esercitazione sono stati creati un `ProductsCL` classe a dimostrazione del funzionamento a livello di codice con la cache dei dati. Per utilizzare le dipendenze della cache SQL nel livello di memorizzazione nella cache, utilizzare il `SqlCacheDependency` classe.

Con il sistema di polling, un `SqlCacheDependency` oggetto deve essere associato a una coppia di database e una tabella particolare. Il codice seguente, ad esempio, crea una `SqlCacheDependency` oggetto basato sul database Northwind s `Products` tabella:


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample10.cs)]

I due parametri di input di `SqlCacheDependency` costruttore s sono i nomi di database e tabella, rispettivamente. Come con gli oggetti ObjectDataSource `SqlCacheDependency` proprietà, il nome del database utilizzato è identico al valore specificato nel `name` attributo delle `<add>` elemento `Web.config`. Il nome della tabella è il nome effettivo della tabella di database.

Per associare un `SqlCacheDependency` con un elemento aggiunto alla cache dei dati, usare uno del `Insert` overload del metodo che accetta una dipendenza. Il codice seguente aggiunge *valore* alla cache dei dati per un periodo di tempo indefinito, ma associa a un `SqlCacheDependency` sul `Products` tabella. In breve *valore* rimarrà nella cache fino a quando non viene eliminato a causa dei vincoli di memoria o perché il sistema di polling ha rilevato che il `Products` tabella è stata modificata dal momento che è stata memorizzata nella cache.


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample11.cs)]

I dispositivi di livello la memorizzazione nella cache `ProductsCL` classe attualmente vengono memorizzati nella cache i dati dal `Products` tabella usando una scadenza basati sul tempo di 60 secondi. Consentire s aggiornare questa classe in modo che utilizzi invece delle dipendenze della cache SQL. Il `ProductsCL` classe s `AddCacheItem` metodo, che è responsabile dell'aggiunta di dati nella cache, attualmente contiene il codice seguente:


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample12.cs)]

Aggiornare il codice per utilizzare un `SqlCacheDependency` dell'oggetto anziché il `MasterCacheKeyArray` della dipendenza dalla cache:


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample13.cs)]

Per testare questa funzionalità, aggiungere un controllo GridView alla pagina sotto esistente `ProductsDeclarative` GridView. Impostare questa nuova s GridView `ID` al `ProductsProgrammatic` e, tramite suo smart tag, associarlo a un nuovo oggetto ObjectDataSource denominato `ProductsDataSourceProgrammatic`. Configurare ObjectDataSource per usare la `ProductsCL` (classe), l'impostazione di elenchi a discesa selezione e le schede di aggiornamento per `GetProducts` e `UpdateProduct`, rispettivamente.


[![Configurare ObjectDataSource per usare la classe ProductsCL](using-sql-cache-dependencies-cs/_static/image11.gif)](using-sql-cache-dependencies-cs/_static/image15.png)

**Figura 11**: configurare ObjectDataSource per usare il `ProductsCL` classe ([fare clic per visualizzare l'immagine con dimensioni normali](using-sql-cache-dependencies-cs/_static/image16.png))


[![Selezionare il metodo GetProducts dall'elenco a discesa s scheda Seleziona](using-sql-cache-dependencies-cs/_static/image12.gif)](using-sql-cache-dependencies-cs/_static/image17.png)

**Figura 12**: selezionare il `GetProducts` metodo da s selezionare scheda Riepilogo ([fare clic per visualizzare l'immagine con dimensioni normali](using-sql-cache-dependencies-cs/_static/image18.png))


[![Scegliere il metodo UpdateProduct dall'elenco a discesa s scheda UPDATE](using-sql-cache-dependencies-cs/_static/image13.gif)](using-sql-cache-dependencies-cs/_static/image19.png)

**Figura 13**: scegliere la scheda aggiornamento s elenco a discesa metodo UpdateProduct ([fare clic per visualizzare l'immagine con dimensioni normali](using-sql-cache-dependencies-cs/_static/image20.png))


Dopo aver completato la procedura guidata Configura origine dati, Visual Studio creerà CheckBoxFields e BoundField in GridView per ognuno dei campi dati. Come in GridView prima aggiunto a questa pagina, rimuovere tutti i campi, ma `ProductName`, `CategoryName`, e `UnitPrice`e formattare i campi nel modo desiderato. GridView s nello smart tag, selezionare le caselle di controllo Abilita Paging, abilitare l'ordinamento e Abilita modifica. Come con le `ProductsDataSourceDeclarative` ObjectDataSource, Visual Studio imposterà il `ProductsDataSourceProgrammatic` s ObjectDataSource `OldValuesParameterFormatString` proprietà `original_{0}`. Affinché la funzionalità di modifica GridView s per il corretto funzionamento, impostare questa proprietà viene ricopiato `{0}` (o rimuovere completamente l'assegnazione di proprietà con la sintassi dichiarativa).

Dopo aver completato queste attività, markup dichiarativo GridView e ObjectDataSource risultante dovrebbe essere simile al seguente:


[!code-aspx[Main](using-sql-cache-dependencies-cs/samples/sample14.aspx)]

Per testare il codice SQL dipendenza della cache nel livello di memorizzazione nella cache imposta un punto di interruzione il `ProductCL` classe s `AddCacheItem` (metodo) e quindi avvia il debug. Quando si visita innanzitutto `SqlCacheDependencies.aspx`, il punto di interruzione verrà raggiunto quando viene richiesto per la prima volta e inseriti nella cache i dati. Successivamente, spostare in un'altra pagina nel GridView o ordinare una delle colonne. In questo modo, il controllo GridView rieseguire una query dei dati, ma devono trovare i dati nella cache dopo il `Products` tabella di database non è stata modificata. Se i dati più volte non viene trovati nella cache, verificare che nel computer sia disponibile memoria sufficiente e ripetere l'operazione.

Dopo il paging di alcune pagine di GridView, aprire una seconda finestra del browser e passare all'Esercitazione nozioni di base di modifica, inserimento ed eliminazione di sezione (`~/EditInsertDelete/Basics.aspx`). Aggiornare un record dalla tabella Products e quindi, nella finestra del browser prima consente di visualizzare una nuova pagina o fare clic su una delle intestazioni di ordinamento.

In questo scenario verrà visualizzato uno dei due cose: entrambi il punto di interruzione verrà raggiunto, che indica che i dati memorizzati nella cache è stati eliminati a causa della modifica nel database. In alternativa, il punto di interruzione non viene raggiunto, vale a dire che `SqlCacheDependencies.aspx` ora mostra i dati non aggiornati. Se non viene raggiunto il punto di interruzione, è probabile perché il servizio di polling non ha ancora generato poiché i dati sono stati modificati. Tenere presente che il servizio di polling è controllo delle modifiche per il `Products` tabella ogni `pollTime` millisecondi, in modo che si verifica un ritardo tra quando i dati sottostanti vengono aggiornati e quando i dati memorizzati nella cache vengano rimosso.

> [!NOTE]
> Questo ritardo è più probabile che venga visualizzato quando si modifica uno dei prodotti tramite il controllo GridView in `SqlCacheDependencies.aspx`. Nel [dati la memorizzazione nella cache nell'architettura](caching-data-in-the-architecture-cs.md) esercitazione è stato aggiunto il `MasterCacheKeyArray` memorizzare nella cache delle dipendenze per garantire che i dati in fase di modifica tramite la `ProductsCL` classe s `UpdateProduct` metodo è stato rimosso dalla cache. Tuttavia, è stata sostituita la dipendenza della cache quando si modifica il `AddCacheItem` metodo più indietro in questo passaggio e pertanto il `ProductsCL` classe Continuerai a visualizzare i dati memorizzati nella cache fino a quando il sistema di polling viene rilevato per la modifica il `Products` tabella. Si vedrà come reintrodurre la `MasterCacheKeyArray` memorizzare nella cache delle dipendenze nel passaggio 7.


## <a name="step-7-associating-multiple-dependencies-with-a-cached-item"></a>Passaggio 7: Associazione di più dipendenze con un elemento memorizzato nella cache

Si tenga presente che il `MasterCacheKeyArray` dipendenza della cache viene usata per verificare che *tutti* dati relativi al prodotto vengano rimosso dalla cache quando viene aggiornato ogni singolo articolo associato all'interno di esso. Ad esempio, il `GetProductsByCategoryID(categoryID)` metodo cache `ProductsDataTables` istanze per ogni univoco *categoryID* valore. Se uno di questi oggetti viene rimosso, il `MasterCacheKeyArray` dipendenza della cache assicura che anche gli altri vengono rimossi. Senza questa dipendenza della cache, quando vengono modificati i dati memorizzati nella cache esiste la possibilità che altri dati memorizzati nella cache del prodotto potrebbero non essere aggiornati. Di conseguenza, è s importante che abbiamo mantenuto la `MasterCacheKeyArray` della dipendenza dalla cache quando si usano le dipendenze della cache SQL. Tuttavia, i dati nella cache s `Insert` metodo consente solo per un oggetto di dipendenza.

Inoltre, quando si lavora con le dipendenze della cache SQL potrà dobbiamo associare più tabelle di database come dipendenze. Ad esempio, il `ProductsDataTable` memorizzato nella cache nel `ProductsCL` classe contiene i nomi di categoria e il fornitore per ogni prodotto, ma la `AddCacheItem` metodo Usa solo una dipendenza su `Products`. In questo caso, se l'utente non aggiorna il nome di una categoria o fornitore, i dati memorizzati nella cache del prodotto rimarranno nella cache e non essere aggiornate. Pertanto, desideriamo rendere i dati memorizzati nella cache del prodotto dipende non solo il `Products` tabella, ma nel `Categories` e `Suppliers` anche le tabelle.

Il [ `AggregateCacheDependency` classe](https://msdn.microsoft.com/library/system.web.caching.aggregatecachedependency.aspx) fornisce un modo per associare più dipendenze di un elemento della cache. Iniziare creando un `AggregateCacheDependency` istanza. Successivamente, aggiungere il set di dipendenze mediante il `AggregateCacheDependency` s `Add` (metodo). Quando si inserisce l'elemento nella cache dati successivamente, passare il `AggregateCacheDependency` istanza. Quando *eventuali* del `AggregateCacheDependency` istanza s dipendenze cambiano, l'elemento memorizzato nella cache verrà rimossi.

Di seguito è riportato il codice aggiornato per il `ProductsCL` classe s `AddCacheItem` (metodo). Il metodo crea il `MasterCacheKeyArray` memorizza nella cache di dipendenza insieme a `SqlCacheDependency` degli oggetti per il `Products`, `Categories`, e `Suppliers` tabelle. Questi metodi vengono combinati tutti in un unico `AggregateCacheDependency` oggetto denominato `aggregateDependencies`, che viene quindi passato il `Insert` (metodo).


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample15.cs)]

Testare il codice nuovo out. Ora diventa le `Products`, `Categories`, o `Suppliers` tabelle causano il rimozione dei dati memorizzati nella cache. Inoltre, il `ProductsCL` classe s `UpdateProduct` metodo, che viene chiamato durante la modifica di un prodotto tramite il controllo GridView, rimuove le `MasterCacheKeyArray` della dipendenza, provocando memorizzato nella cache dalla cache `ProductsDataTable` rimozione e i dati da recuperare nuovamente nella successiva richiesta.

> [!NOTE]
> Dipendenze della cache SQL possono anche essere utilizzate con [memorizzazione nella cache di output](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/caching/output.aspx). Per una dimostrazione di questa funzionalità, vedere: [Using Caching di Output ASP.NET con SQL Server](https://msdn.microsoft.com/library/e3w8402y(VS.80).aspx).


## <a name="summary"></a>Riepilogo

Quando la memorizzazione nella cache i dati del database, i dati in una situazione ideale rimarrà nella cache fino a quando non viene modificata nel database. Con ASP.NET 2.0, le dipendenze della cache SQL possono essere create e usate in scenari sia strumenti dichiarativi e programmatici. Una delle difficoltà con questo approccio è scoprire quando sono stati modificati i dati. Le versioni complete di Microsoft SQL Server 2005 offrono funzionalità di notifica che è un'applicazione avvisa in caso di un risultato della query è stato modificato. Per la Express Edition di SQL Server 2005 e versioni precedenti di SQL Server, è necessario utilizzare invece un sistema di polling. Fortunatamente, configurare l'infrastruttura necessaria di polling è piuttosto semplice.

Buona programmazione!

## <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Tramite le notifiche delle Query in Microsoft SQL Server 2005](https://msdn.microsoft.com/library/ms175110.aspx)
- [Creazione di una notifica delle Query](https://msdn.microsoft.com/library/ms188669.aspx)
- [La memorizzazione nella cache in ASP.NET con il `SqlCacheDependency` classe](https://msdn.microsoft.com/library/ms178604(VS.80).aspx)
- [Strumento di registrazione di ASP.NET SQL Server (`aspnet_regsql.exe`)](https://msdn.microsoft.com/library/ms229862(vs.80).aspx)
- [Panoramica di `SqlCacheDependency`](http://www.aspnetresources.com/blog/sql_cache_depedency_overview.aspx)

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. I revisori per questa esercitazione sono state Marko Rangel Teresa Murphy e Hilton Giesenow. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](caching-data-at-application-startup-cs.md)
> [Successivo](caching-data-with-the-objectdatasource-vb.md)
