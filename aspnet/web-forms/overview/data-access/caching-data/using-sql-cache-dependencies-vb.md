---
uid: web-forms/overview/data-access/caching-data/using-sql-cache-dependencies-vb
title: Utilizzando le dipendenze della Cache SQL (VB) | Documenti Microsoft
author: rick-anderson
description: La strategia di memorizzazione nella cache più semplice consiste nel consentire i dati memorizzati nella cache scada dopo un periodo di tempo specificato. Tuttavia, questo approccio semplice significa che i dati memorizzati nella cache di maintai...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2007
ms.topic: article
ms.assetid: bd347d93-4251-4532-801c-a36f2dfa7f96
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/caching-data/using-sql-cache-dependencies-vb
msc.type: authoredcontent
ms.openlocfilehash: 452d856fe352ef2eb7dfcc3f3acd6aa5bcb5ae41
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878373"
---
<a name="using-sql-cache-dependencies-vb"></a>Utilizzando le dipendenze della Cache SQL (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare codice](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_61_VB.zip) o [Scarica il PDF](using-sql-cache-dependencies-vb/_static/datatutorial61vb1.pdf)

> La strategia di memorizzazione nella cache più semplice consiste nel consentire i dati memorizzati nella cache scada dopo un periodo di tempo specificato. Ma questo approccio semplice significa che i dati memorizzati nella cache non gestisce nessuna associazione con l'origine dati sottostante, dando luogo a dati non aggiornati che viene mantenuti troppo lungo o corrente dei dati che sono scaduti troppo presto. Un approccio migliore consiste nell'utilizzare la classe SqlCacheDependency in modo che i dati rimangono memorizzati nella cache fino a quando i dati sottostanti sono stati modificati nel database SQL. In questa esercitazione verrà illustrato.


## <a name="introduction"></a>Introduzione

Esaminate le tecniche di memorizzazione nella cache il [la memorizzazione nella cache di dati con ObjectDataSource](caching-data-with-the-objectdatasource-vb.md) e [memorizzazione dei dati nell'architettura](caching-data-in-the-architecture-vb.md) esercitazioni consentono di rimuovere i dati dalla cache dopo un determinato una scadenza basati sull'ora periodo. Questo approccio è il modo più semplice per bilanciare il miglioramento delle prestazioni di memorizzazione nella cache con obsolescenza dei dati. Selezionando una scadenza del tempo di *x* secondi, lo sviluppatore della pagina concedes per sfruttare i vantaggi delle prestazioni di memorizzazione nella cache solo per *x* secondi, ma è possibile posizionare semplice che suo dati non potranno mai essere non aggiornati più tempo rispetto a un massimo di *x* secondi. Naturalmente, per i dati statici, *x* può essere esteso al ciclo di vita dell'applicazione web, come è stato esaminato nel [memorizzazione dei dati all'avvio dell'applicazione](caching-data-at-application-startup-vb.md) esercitazione.

Quando la memorizzazione nella cache dei dati del database, una scadenza basati sull'ora è spesso scelti per la facilità d'uso, ma spesso una soluzione insufficiente. In teoria, i dati del database resta memorizzato nella cache fino a quando i dati sottostanti sono stati modificati nel database. solo a questo punto verrà eliminata la cache. Questo approccio consente di ottimizzare le prestazioni di memorizzazione nella cache e riduce al minimo la durata dei dati non aggiornati. Tuttavia, per poter sfruttare questi vantaggi sono deve essere un sistema che sa quando i dati del database sottostante sono stati modificati e rimuove gli elementi corrispondenti dalla cache. Prima di ASP.NET 2.0, gli sviluppatori di pagina sono responsabili dell'implementazione di questo sistema.

ASP.NET 2.0 fornisce un [ `SqlCacheDependency` classe](https://msdn.microsoft.com/library/system.web.caching.sqlcachedependency.aspx) e l'infrastruttura necessaria per determinare quando è stata modificata nel database in modo che i corrispondenti elementi memorizzati nella cache può essere rimossa. Sono disponibili due tecniche per determinare quando i dati sottostanti vengono modificati: notifica e il polling. Dopo che illustrano le differenze tra la notifica e polling, si creerà l'infrastruttura necessaria supportare il polling e quindi illustrato come usare il `SqlCacheDependency` scenari classe nella sintassi dichiarativa e a livello di codice.

## <a name="understanding-notification-and-polling"></a>Il polling e informazioni sulle notifiche

Sono disponibili due tecniche che possono essere utilizzate per determinare se i dati in un database sono stati modificati: notifica e il polling. Con la notifica, il database dal runtime ASP.NET viene avvisato automaticamente se i risultati di una particolare query sono stati modificati dopo l'ultima esecuzione della query, a quel punto vengono rimossi gli elementi memorizzati associati alla query. Con il polling, il server di database mantiene informazioni quando particolari tabelle ultimo sono state aggiornate. Il runtime di ASP.NET esegue periodicamente il polling del database per controllare quali le tabelle sono state poiché sono stati immessi nella cache. Tali tabelle i cui dati sono stati modificati disporre gli elementi della cache associata rimossi.

L'opzione di notifica richiede meno il programma di installazione di polling e più granulare poiché tiene traccia delle modifiche a livello di query anziché a livello di tabella. Purtroppo, le notifiche sono disponibili solo nelle edizioni complete di Microsoft SQL Server 2005 (ad esempio, le edizioni non Express). Tuttavia, è possibile utilizzare l'opzione di polling per tutte le versioni di Microsoft SQL Server da 7.0 a 2005. Poiché tali esercitazioni vengono utilizzati l'edizione Express di SQL Server 2005, si concentra sulla configurazione e l'utilizzo dell'opzione di polling. Alla fine di questa esercitazione per ulteriori risorse sulle funzionalità di notifica di SQL Server 2005 s, consultare la sezione ulteriormente la lettura.

Con il polling, il database deve essere configurato per includere una tabella denominata `AspNet_SqlCacheTablesForChangeNotification` con tre colonne: `tableName`, `notificationCreated`, e `changeId`. Questa tabella contiene una riga per ogni tabella che dispone di dati che potrebbe essere necessario utilizzare una dipendenza della cache SQL nell'applicazione web. Il `tableName` colonna specifica il nome della tabella durante `notificationCreated` indica la data e ora la riga è stata aggiunta alla tabella. Il `changeId` colonna è di tipo `int` e ha un valore iniziale pari a 0. Il valore viene incrementato ad ogni modifica alla tabella.

Oltre al `AspNet_SqlCacheTablesForChangeNotification` tabella, il database deve inoltre includere trigger definiti per le tabelle che possono essere visualizzati in una dipendenza della cache SQL. Questi trigger vengono eseguiti ogni volta che una riga viene inserita, aggiornata o eliminata e incrementare la tabella s `changeId` valore `AspNet_SqlCacheTablesForChangeNotification`.

Il runtime di ASP.NET tiene traccia corrente `changeId` per una tabella quando la memorizzazione nella cache di dati mediante un `SqlCacheDependency` oggetto. Il database viene periodicamente controllato e qualsiasi `SqlCacheDependency` oggetti la cui proprietà `changeId` è diverso dal valore nel database vengono eliminati dopo che un differisce `changeId` valore indica che si è verificata una modifica alla tabella perché è stato memorizzato nella cache i dati.

## <a name="step-1-exploring-theaspnetregsqlexecommand-line-program"></a>Passaggio 1: Esplorazione di`aspnet_regsql.exe`programma della riga di comando

Con l'approccio di polling, il database deve essere configurato per contenere l'infrastruttura descritto in precedenza: una tabella predefinita (`AspNet_SqlCacheTablesForChangeNotification`), un numero limitato di stored procedure e trigger definiti per le tabelle che possono essere utilizzate nelle dipendenze della cache SQL nel sito web applicazione. Queste tabelle, stored procedure e trigger può essere creato tramite il programma della riga di comando `aspnet_regsql.exe`, che si trovano nel `$WINDOWS$\Microsoft.NET\Framework\version` cartella. Per creare il `AspNet_SqlCacheTablesForChangeNotification` tabella e stored procedure associate, eseguire il comando seguente dalla riga di comando:


[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample1.cmd)]

> [!NOTE]
> Per eseguire questi comandi di accesso al database specificato deve essere il [ `db_securityadmin` ](https://msdn.microsoft.com/library/ms188685.aspx) e [ `db_ddladmin` ](https://msdn.microsoft.com/library/ms190667.aspx) ruoli. Per esaminare l'oggetto T-SQL inviate al database per il `aspnet_regsql.exe` programma della riga di comando, fare riferimento a [questo post di blog](http://scottonwriting.net/sowblog/posts/10709.aspx).


Ad esempio, per aggiungere l'infrastruttura per il polling a un database di Microsoft SQL Server denominato `pubs` in un server di database denominato `ScottsServer` utilizzando l'autenticazione di Windows, passare alla directory appropriata e, dalla riga di comando, immettere:


[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample2.cmd)]

Dopo aver aggiunto l'infrastruttura a livello di database, è necessario aggiungere i trigger per le tabelle che saranno utilizzate in dipendenze della cache SQL. Utilizzare il `aspnet_regsql.exe` riga di comando nuovo del programma, ma specificare il nome di tabella utilizzando il `-t` passare e invece di usare il `-ed` passare utilizzare `-et`, come illustrato di seguito:


[!code-html[Main](using-sql-cache-dependencies-vb/samples/sample3.html)]

Per aggiungere i trigger per il `authors` e `titles` tabelle nel `pubs` database `ScottsServer`, utilizzare:


[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample4.cmd)]

Per questa esercitazione, aggiungere i trigger per il `Products`, `Categories`, e `Suppliers` tabelle. Esamineremo la sintassi della riga di comando specifico nel passaggio 3.

## <a name="step-2-referencing-a-microsoft-sql-server-2005-express-edition-database-inappdata"></a>Passaggio 2: Riferimento a un Database Microsoft SQL Server 2005 Express Edition in`App_Data`

Il `aspnet_regsql.exe` programma della riga di comando richiede il nome del database e server per aggiungere l'infrastruttura di polling necessarie. Ma qual è il nome di database e server per un database Microsoft SQL Server 2005 Express in cui si trova il `App_Data` cartella? Anziché dover individuare quali sono i nomi di database e server, si ve ha rilevato che l'approccio più semplice per collegare il database per il `localhost\SQLExpress` istanza del database e rinominare i dati utilizzando [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx). Se si dispone di una delle versioni complete di SQL Server 2005 installati nel computer, quindi probabile che abbiano già installati nel computer SQL Server Management Studio. Se si dispone solo di Web Developer Express edition, è possibile scaricare la versione gratuita [Microsoft SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?displaylang=en&amp;FamilyID=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796).

Per iniziare, chiudere Visual Studio. Quindi, aprire SQL Server Management Studio e scegliere di connettersi il `localhost\SQLExpress` server utilizzando l'autenticazione di Windows.


![Collegare a localhost\SQLExpress Server](using-sql-cache-dependencies-vb/_static/image1.gif)

**Figura 1**: collegare il `localhost\SQLExpress` Server


Dopo la connessione al server Management Studio verranno Mostra il server e contiene sottocartelle per il database, sicurezza e così via. Pulsante destro del mouse sulla cartella database e scegliere l'opzione di connessione. Verrà visualizzata la finestra di dialogo Collega database (vedere la figura 2). Fare clic sul pulsante Aggiungi e selezionare il `NORTHWND.MDF` cartella del database nella raccolta di s di applicazione web `App_Data` cartella.


[![Collegare il NORTHWND. Database MDF dalla cartella App_Data](using-sql-cache-dependencies-vb/_static/image2.gif)](using-sql-cache-dependencies-vb/_static/image1.png)

**Figura 2**: collegare il `NORTHWND.MDF` del Database del `App_Data` cartella ([fare clic per visualizzare l'immagine ingrandita](using-sql-cache-dependencies-vb/_static/image2.png))


Il database verrà aggiunta nella cartella di database. Il nome del database potrebbe essere il percorso completo del file di database o il percorso completo preceduto da un [GUID](http://en.wikipedia.org/wiki/Globally_Unique_Identifier). Per evitare di dover digitare il nome lungo del database quando si utilizza aspnet\_regsql.exe strumento da riga di comando, rinominare il database a un nome più adatto al umane facendo clic sul database appena collegato e scegliendo Rinomina. I database ve rinominato DataTutorials.


![Rinominare il Database collegato con un nome più adatto al personale](using-sql-cache-dependencies-vb/_static/image3.gif)

**Figura 3**: rinominare il Database collegato con un nome più adatti all'umane


## <a name="step-3-adding-the-polling-infrastructure-to-the-northwind-database"></a>Passaggio 3: Aggiunta dell'infrastruttura di Polling per il Database Northwind

Ora che è stata associata la `NORTHWND.MDF` del database del `App_Data` cartella, è nuovamente pronto per aggiungere l'infrastruttura di polling. Supponendo che il database è già rinominata DataTutorials, eseguire i seguenti quattro comandi:


[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample5.cmd)]

Dopo aver eseguito questi quattro comandi, fare doppio clic sul nome del database in Management Studio, passare al sottomenu attività e scegliere Disconnetti. Management Studio chiudere e riaprire Visual Studio.

Una volta è riaperto Visual Studio, esaminare i database tramite Esplora Server. Si noti la nuova tabella (`AspNet_SqlCacheTablesForChangeNotification`), la nuova stored procedure e trigger nel `Products`, `Categories`, e `Suppliers` tabelle.


![Il Database include ora l'infrastruttura necessaria Polling](using-sql-cache-dependencies-vb/_static/image4.gif)

**Figura 4**: il Database include ora l'infrastruttura necessaria Polling


## <a name="step-4-configuring-the-polling-service"></a>Passaggio 4: Configurazione del servizio di Polling

Dopo aver creato le tabelle necessarie, trigger e stored procedure nel database, il passaggio finale consiste nel configurare il servizio di polling, che viene eseguito tramite `Web.config` specificando i database da utilizzare e la frequenza di polling espresso in millisecondi. Il markup seguente esegue il polling del database Northwind, una volta al secondo.


[!code-xml[Main](using-sql-cache-dependencies-vb/samples/sample6.xml)]

Il `name` valore il `<add>` elemento (NorthwindDB) consente di associare un nome leggibile dall'utente con un determinato database. Quando si utilizzano le dipendenze della cache SQL, è necessario fare riferimento al nome del database definito qui e la tabella basata su dati memorizzati nella cache. Si vedrà come utilizzare la `SqlCacheDependency` classe da associare a livello di codice SQL le dipendenze della cache nella cache i dati nel passaggio 6.

Una volta stabilita una dipendenza della cache SQL, il sistema di polling si connetterà ai database di cui è definiti nel `<databases>` elementi ogni `pollTime` millisecondi ed eseguire il `AspNet_SqlCachePollingStoredProcedure` stored procedure. Eseguire il backup di questa stored procedure - è stato aggiunto nel passaggio 3 utilizzando il `aspnet_regsql.exe` strumento da riga di comando - restituisce il `tableName` e `changeId` i valori per ogni record `AspNet_SqlCacheTablesForChangeNotification`. Le dipendenze della cache SQL non aggiornate vengono rimossi dalla cache.

Il `pollTime` impostazione introduce un compromesso tra prestazioni e obsolescenza dei dati. Una piccola `pollTime` valore aumenta il numero di richieste al database, ma più rapidamente rimuove i dati non aggiornati dalla cache. Una maggiore `pollTime` valore riduce il numero di richieste al database, ma aumenta il ritardo tra quando i dati di back-end cambiano e quando vengono eliminati gli elementi correlati. Fortunatamente, la richiesta di database viene eseguita una stored procedure semplice che s restituzione solo poche righe di una tabella semplice e leggera. Ma sperimentare diverse `pollTime` i valori per trovare un equilibrio perfetto tra database obsolescenza dei dati e di accesso per l'applicazione. Il più piccolo `pollTime` valore consentito è 500.

> [!NOTE]
> Nell'esempio sopra riportato fornisce un singolo `pollTime` valore nel `<sqlCacheDependency>` elemento, ma è possibile specificare facoltativamente il `pollTime` valore il `<add>` elemento. Ciò è utile se si dispone di più database specificati e si desidera personalizzare la frequenza di polling per ogni database.


## <a name="step-5-declaratively-working-with-sql-cache-dependencies"></a>Passaggio 5: Utilizzo in modo dichiarativo le dipendenze della Cache SQL

In passaggi da 1 a 4 è stato illustrato come configurare l'infrastruttura del database necessari e configurare il sistema di polling. Con questa infrastruttura, possiamo ora aggiungere elementi alla cache di dati con una dipendenza della cache SQL associata utilizzando tecniche a livello di codice o dichiarative. In questo passaggio verrà esaminato come utilizzare in modo dichiarativo con le dipendenze della cache SQL. Nel passaggio 6 verrà esaminato l'approccio a livello di codice.

Il [la memorizzazione nella cache di dati con ObjectDataSource](caching-data-with-the-objectdatasource-vb.md) esercitazione esplorata le funzionalità di memorizzazione nella cache dichiarative di ObjectDataSource. Impostando semplicemente il `EnableCaching` proprietà `True` e `CacheDuration` proprietà per un intervallo di tempo, ObjectDataSource inserirà automaticamente nella cache i dati restituiti dal relativo oggetto sottostante per l'intervallo specificato. ObjectDataSource è inoltre possibile utilizzare uno o più dipendenze della cache SQL.

Per illustrare l'utilizzo in modo dichiarativo le dipendenze della cache SQL, aprire il `SqlCacheDependencies.aspx` nella pagina di `Caching` cartelle e trascinare un controllo GridView dalla casella degli strumenti nella finestra di progettazione. Impostare i dispositivi di GridView `ID` a `ProductsDeclarative` e dal suo smart tag scegliere da associare a un nuovo oggetto ObjectDataSource denominato `ProductsDataSourceDeclarative`.


[![Creare un nuovo oggetto ObjectDataSource denominato ProductsDataSourceDeclarative](using-sql-cache-dependencies-vb/_static/image5.gif)](using-sql-cache-dependencies-vb/_static/image3.png)

**Figura 5**: creare un nuovo denominato ObjectDataSource `ProductsDataSourceDeclarative` ([fare clic per visualizzare l'immagine ingrandita](using-sql-cache-dependencies-vb/_static/image4.png))


Configurare ObjectDataSource per utilizzare il `ProductsBLL` classe e impostare l'elenco a discesa nella scheda Seleziona per `GetProducts()`. Nella scheda di aggiornamento, scegliere il `UpdateProduct` eseguire l'overload con tre parametri di input - `productName`, `unitPrice`, e `productID`. Impostare gli elenchi a discesa su (nessuno) nelle schede delle INSERT e DELETE.


[![Utilizzare l'Overload UpdateProduct con tre parametri di Input](using-sql-cache-dependencies-vb/_static/image6.gif)](using-sql-cache-dependencies-vb/_static/image5.png)

**Figura 6**: usare l'Overload UpdateProduct con tre parametri di Input ([fare clic per visualizzare l'immagine ingrandita](using-sql-cache-dependencies-vb/_static/image6.png))


[![Impostazione dell'elenco di riepilogo a discesa da (nessuno) per l'INSERT e DELETE schede](using-sql-cache-dependencies-vb/_static/image7.gif)](using-sql-cache-dependencies-vb/_static/image7.png)

**Figura 7**: impostare l'elenco a discesa su (nessuno) per di inserimento ed eliminazione schede ([fare clic per visualizzare l'immagine ingrandita](using-sql-cache-dependencies-vb/_static/image8.png))


Dopo aver completato la configurazione guidata origine dati, Visual Studio creerà BoundField e CheckBoxFields in GridView per ognuno dei campi dati. Rimuovere tutti i campi ma `ProductName`, `CategoryName`, e `UnitPrice`e formattare questi campi in base alle esigenze. Dalla GridView s smart tag, selezionare le caselle di controllo Abilita Paging, abilitare ordinamento e abilitare la modifica. Visual Studio verrà impostato il s ObjectDataSource `OldValuesParameterFormatString` proprietà `original_{0}`. Affinché la funzionalità di modifica GridView s per il corretto funzionamento, rimuovere la proprietà completamente dalla sintassi dichiarativa o impostarla di nuovo il valore predefinito `{0}`.

Infine, aggiungere un controllo etichetta Web di sopra del controllo GridView e impostare il relativo `ID` proprietà `ODSEvents` e il relativo `EnableViewState` proprietà `False`. Dopo aver apportato queste modifiche, markup dichiarativo s pagina dovrebbe essere simile al seguente. Si noti che se va eseguito un numero di personalizzazioni estetici ai campi GridView che non sono necessari per dimostrare la funzionalità di dipendenza della cache SQL.


[!code-aspx[Main](using-sql-cache-dependencies-vb/samples/sample7.aspx)]

Successivamente, creare un gestore eventi per ObjectDataSource s `Selecting` eventi e nell'aggiungere il codice seguente:


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample8.vb)]

Tenere presente che i dispositivi ObjectDataSource `Selecting` evento viene generato solo quando si recuperano dati dal relativo oggetto sottostante. Se ObjectDataSource accede ai dati dalla cache, non viene generato questo evento.

A questo punto, visitare la pagina tramite un browser. Poiché è ve ancora per implementare qualsiasi la memorizzazione nella cache, ogni volta che si pagina, ordinare o modificare la griglia della pagina deve essere visualizzato il testo, l'evento Selecting generato, come mostrato nella figura 8.


[![S ObjectDataSource evento Selecting viene generato ogni volta che il controllo GridView viene eseguito il paging, modificare, o Sorted](using-sql-cache-dependencies-vb/_static/image8.gif)](using-sql-cache-dependencies-vb/_static/image9.png)

**Figura 8**: il ObjectDataSource s `Selecting` evento viene attivato ogni ora GridView viene eseguito il paging, modificato o Sorted ([fare clic per visualizzare l'immagine ingrandita](using-sql-cache-dependencies-vb/_static/image10.png))


Come illustrato nel [la memorizzazione nella cache di dati con ObjectDataSource](caching-data-with-the-objectdatasource-vb.md) esercitazione, impostare il `EnableCaching` proprietà `True` provoca ObjectDataSource memorizzare nella cache i dati per la durata specificata dal relativo `CacheDuration` proprietà. ObjectDataSource ha anche un [ `SqlCacheDependency` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.sqlcachedependency.aspx), che aggiunge uno o più dipendenze della cache SQL per i dati memorizzati nella cache usando il modello:


[!code-css[Main](using-sql-cache-dependencies-vb/samples/sample9.css)]

Dove *databaseName* è il nome del database come specificato nella `name` attributo del `<add>` elemento `Web.config`, e *tableName* è il nome della tabella di database. Ad esempio, per creare un ObjectDataSource che memorizza nella cache di dati per un periodo illimitato basata su una dipendenza della cache SQL in s Northwind `Products` tabella, impostare il s ObjectDataSource `EnableCaching` proprietà `True` e il relativo `SqlCacheDependency` proprietà NorthwindDB:Products.

> [!NOTE]
> È possibile utilizzare una dipendenza della cache SQL *e* una scadenza basati sul tempo impostando `EnableCaching` a `True`, `CacheDuration` per l'intervallo di tempo e `SqlCacheDependency` per i nomi di database e tabella. ObjectDataSource rimuoverà i dati quando viene raggiunta la scadenza basati sul tempo o quando il sistema di polling indica che i dati del database sottostante è cambiato, a seconda del valore si verifica per prima.


GridView in `SqlCacheDependencies.aspx` Visualizza i dati di due tabelle - `Products` e `Categories` (prodotto s `CategoryName` campo viene recuperato tramite un `JOIN` su `Categories`). Pertanto, si desidera specificare le dipendenze della cache di due SQL: NorthwindDB:Products; NorthwindDB:Categories.


[![Configurare ObjectDataSource per supportare la memorizzazione nella cache usando le dipendenze della Cache SQL sui prodotti e le categorie](using-sql-cache-dependencies-vb/_static/image9.gif)](using-sql-cache-dependencies-vb/_static/image11.png)

**Figura 9**: configurare ObjectDataSource a supporto di memorizzazione nella cache con dipendenze della Cache SQL nel `Products` e `Categories` ([fare clic per visualizzare l'immagine ingrandita](using-sql-cache-dependencies-vb/_static/image12.png))


Dopo aver configurato il ObjectDataSource per supportare la memorizzazione nella cache, rivedere la pagina tramite un browser. L'evento di selezione di testo generato nuovamente, dovrebbe essere visualizzato nella prima visita pagina, ma non verrà più visualizzato quando il paging, l'ordinamento o fare clic sul pulsante di modifica o annullamento. Infatti, dopo che i dati vengono caricati nella cache ObjectDataSource s, rimane presente finché non il `Products` o `Categories` tabelle vengono modificate o i dati vengono aggiornati tramite il controllo GridView.

Dopo lo scorrimento della griglia e notare l'assenza dell'evento Selecting generato testo, aprire una nuova finestra del browser e passare all'Esercitazione nozioni di base in modalità di modifica, inserimento ed eliminazione di sezione (`~/EditInsertDelete/Basics.aspx`). Aggiornare il nome o il prezzo di un prodotto. Quindi, alla prima finestra del browser, visualizzare una pagina diversa dei dati, ordinare la griglia o fare clic su un pulsante Modifica riga s. Questa volta, l'evento Selecting generato deve si ripresenta, come il database sottostante dei dati è stata modificata (vedere Figura 10). Se non viene visualizzato il testo, attendere qualche minuto e riprovare. Tenere presente che il servizio di polling è il controllo per le modifiche di `Products` tabella ogni `pollTime` millisecondi, pertanto si verifica un ritardo tra quando i dati sottostanti vengono aggiornati e quando viene eliminati i dati memorizzati nella cache.


[![Modifica la tabella Products rimuove i dati memorizzati nella cache del prodotto](using-sql-cache-dependencies-vb/_static/image10.gif)](using-sql-cache-dependencies-vb/_static/image13.png)

**Figura 10**: la modifica della tabella Products rimuove i dati di prodotto memorizzati nella cache ([fare clic per visualizzare l'immagine ingrandita](using-sql-cache-dependencies-vb/_static/image14.png))


## <a name="step-6-programmatically-working-with-thesqlcachedependencyclass"></a>Passaggio 6: Utilizzo a livello di codice con la`SqlCacheDependency`classe

Il [memorizzazione dei dati nell'architettura](caching-data-in-the-architecture-vb.md) esercitazione esaminato i vantaggi dell'utilizzo di un livello separato di memorizzazione nella cache dell'architettura della differenza che consente di connettere la memorizzazione nella cache con ObjectDataSource. In tale esercitazione è stato creato un `ProductsCL` classe per illustrare l'utilizzo a livello di codice con la cache dei dati. Per utilizzare le dipendenze della cache SQL nel livello di memorizzazione nella cache, utilizzare la `SqlCacheDependency` classe.

Con il sistema di polling, un `SqlCacheDependency` oggetto deve essere associato a una particolare coppia di database e tabella. Il codice seguente, ad esempio, crea un `SqlCacheDependency` oggetto basato sul database Northwind s `Products` tabella:


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample10.vb)]

I due parametri di input con il `SqlCacheDependency` costruttore s sono i nomi di database e tabella, rispettivamente. Come con i dispositivi ObjectDataSource `SqlCacheDependency` proprietà, il nome del database utilizzato è identico al valore specificato nel `name` attributo del `<add>` elemento `Web.config`. Il nome della tabella è il nome effettivo della tabella di database.

Per associare un `SqlCacheDependency` con un elemento aggiunto alla cache dei dati, utilizzare uno del `Insert` overload del metodo che accetta una dipendenza. Il codice seguente aggiunge *valore* nella cache di dati per una durata di tempo indefinita, ma associa a un `SqlCacheDependency` sul `Products` tabella. In breve, *valore* rimane nella cache fino a quando non viene eliminato a causa dei vincoli di memoria o perché il sistema di polling ha rilevato che il `Products` tabella è stata modificata poiché è stato memorizzato nella cache.


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample11.vb)]

I dispositivi di memorizzazione nella cache di livello `ProductsCL` classe attualmente memorizza nella cache dati dal `Products` tabella con una scadenza basati sul tempo di 60 secondi. Consente di aggiornare questa classe in modo che utilizzi invece le dipendenze della cache SQL s. Il `ProductsCL` classe s `AddCacheItem` metodo, che è responsabile dell'aggiunta dei dati nella cache, attualmente contiene il codice seguente:


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample12.vb)]

Aggiornare il codice per utilizzare un `SqlCacheDependency` oggetto anziché il `MasterCacheKeyArray` della dipendenza dalla cache:


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample13.vb)]

Per testare questa funzionalità, aggiungere un controllo GridView alla pagina sotto esistente `ProductsDeclarative` GridView. Impostare questo nuovo s GridView `ID` a `ProductsProgrammatic` e, tramite il relativo smart tag, associarlo a un nuovo oggetto ObjectDataSource denominato `ProductsDataSourceProgrammatic`. Configurare ObjectDataSource per utilizzare il `ProductsCL` (classe), l'impostazione di elenchi a discesa selezionare e le schede di aggiornamento per `GetProducts` e `UpdateProduct`, rispettivamente.


[![Configurare ObjectDataSource per utilizzare la classe ProductsCL](using-sql-cache-dependencies-vb/_static/image11.gif)](using-sql-cache-dependencies-vb/_static/image15.png)

**Figura 11**: configurare ObjectDataSource per usare il `ProductsCL` classe ([fare clic per visualizzare l'immagine ingrandita](using-sql-cache-dependencies-vb/_static/image16.png))


[![Selezionare il metodo GetProducts dall'elenco a discesa s scegliere scheda](using-sql-cache-dependencies-vb/_static/image12.gif)](using-sql-cache-dependencies-vb/_static/image17.png)

**Figura 12**: selezionare il `GetProducts` metodo da s scheda selezionare nell'elenco ([fare clic per visualizzare l'immagine ingrandita](using-sql-cache-dependencies-vb/_static/image18.png))


[![Selezionare il metodo UpdateProduct dall'elenco a discesa aggiornamento scheda s](using-sql-cache-dependencies-vb/_static/image13.gif)](using-sql-cache-dependencies-vb/_static/image19.png)

**Figura 13**: scegliere il metodo UpdateProduct da s aggiornamento scheda elenco a discesa ([fare clic per visualizzare l'immagine ingrandita](using-sql-cache-dependencies-vb/_static/image20.png))


Dopo aver completato la configurazione guidata origine dati, Visual Studio creerà BoundField e CheckBoxFields in GridView per ognuno dei campi dati. Ad esempio con il primo GridView aggiunti in questa pagina, rimuovere tutti i campi ma `ProductName`, `CategoryName`, e `UnitPrice`e formattare questi campi in base alle esigenze. Dalla GridView s smart tag, selezionare le caselle di controllo Abilita Paging, abilitare ordinamento e abilitare la modifica. Come con la `ProductsDataSourceDeclarative` ObjectDataSource, Visual Studio verrà impostato il `ProductsDataSourceProgrammatic` ObjectDataSource s `OldValuesParameterFormatString` proprietà `original_{0}`. Affinché la funzionalità di modifica GridView s per il corretto funzionamento, impostare questa proprietà eseguire `{0}` (o rimuovere l'assegnazione di proprietà con la sintassi dichiarativa completamente).

Dopo aver completato queste attività, il markup dichiarativo GridView e ObjectDataSource risulta dovrebbe essere simile al seguente:


[!code-aspx[Main](using-sql-cache-dependencies-vb/samples/sample14.aspx)]

Per testare il codice SQL dipendenza della cache nel livello di memorizzazione nella cache imposta un punto di interruzione nel `ProductCL` classe s `AddCacheItem` metodo e quindi avvia il debug. Quando si visita prima `SqlCacheDependencies.aspx`, il punto di interruzione verrà raggiunto come dati sono richiesto per la prima volta e inseriti nella cache. Successivamente, spostare in un'altra pagina in GridView o ordinare una delle colonne. In questo modo, il controllo GridView rieseguire una query dei dati, ma devono trovare i dati nella cache dopo il `Products` tabella di database non è stata modificata. Se i dati più volte non viene trovati nella cache, assicurarsi che nel computer sia disponibile memoria sufficiente e ripetere l'operazione.

Dopo aver paging alcune pagine di GridView, aprire una seconda finestra del browser e passare all'Esercitazione nozioni di base in modalità di modifica, inserimento ed eliminazione di sezione (`~/EditInsertDelete/Basics.aspx`). Aggiornare un record dalla tabella Products e quindi, dalla finestra del browser prima di visualizzare una nuova pagina oppure fare clic su una delle intestazioni di ordinamento.

In questo scenario verrà visualizzato uno dei due elementi: un punto di interruzione verrà raggiunto, che indica che i dati memorizzati nella cache è stati eliminati a causa della modifica nel database. In alternativa, il punto di interruzione non verrà raggiunto, il che significa che `SqlCacheDependencies.aspx` è ora in dati non aggiornati. Se non è stato raggiunto il punto di interruzione, è probabile perché il servizio di polling non ancora generato dalla modifica dei dati. Tenere presente che il servizio di polling è il controllo per le modifiche di `Products` tabella ogni `pollTime` millisecondi, pertanto si verifica un ritardo tra quando i dati sottostanti vengono aggiornati e quando viene eliminati i dati memorizzati nella cache.

> [!NOTE]
> Questo ritardo è più probabile che venga visualizzato quando si modifica uno dei prodotti tramite GridView in `SqlCacheDependencies.aspx`. Nel [memorizzazione dei dati nell'architettura](caching-data-in-the-architecture-vb.md) esercitazione è stato aggiunto il `MasterCacheKeyArray` della dipendenza per garantire che i dati da modificare tramite dalla cache di `ProductsCL` classe s `UpdateProduct` metodo è stato rimosso dalla cache. Tuttavia, pertanto abbiamo sostituito questa dipendenza della cache quando si modifica il `AddCacheItem` metodo più indietro in questo passaggio e pertanto il `ProductsCL` classe continuerà a visualizzare i dati memorizzati nella cache fino a quando il sistema di polling note per la modifica di `Products` tabella. Vedremo come ripristinare il `MasterCacheKeyArray` dipendenza nel passaggio 7 di memorizzare nella cache.


## <a name="step-7-associating-multiple-dependencies-with-a-cached-item"></a>Passaggio 7: Associazione di più dipendenze con un elemento memorizzato nella cache

Tenere presente che il `MasterCacheKeyArray` dipendenza della cache viene utilizzata per garantire che *tutti* dati relativi al prodotto viene rimossa dalla cache quando viene aggiornato un elemento associato all'interno di esso. Ad esempio, il `GetProductsByCategoryID(categoryID)` metodo cache `ProductsDataTables` istanze per ogni univoco *categoryID* valore. Se uno di questi oggetti viene rimosso, il `MasterCacheKeyArray` dipendenza della cache garantisce che gli altri vengono rimossi. Senza questa dipendenza della cache, quando vengono modificati i dati memorizzati nella cache esiste la possibilità che altri dati memorizzati nella cache del prodotto potrebbero non essere aggiornati. Di conseguenza, è importante mantenere è s il `MasterCacheKeyArray` della dipendenza dalla cache, quando si utilizzano le dipendenze della cache SQL. Tuttavia, i dati nella cache s `Insert` metodo consente solo per un oggetto di dipendenza.

Inoltre, quando si lavora con le dipendenze della cache SQL è possibile che sia necessario associare più tabelle di database come dipendenze. Ad esempio, il `ProductsDataTable` nella cache il `ProductsCL` classe contiene i nomi di categoria e il fornitore per ogni prodotto, ma la `AddCacheItem` metodo utilizza solo una dipendenza su `Products`. In questo caso, se l'utente aggiorna il nome di una categoria o il fornitore, i dati memorizzati nella cache del prodotto rimarrà nella cache e non essere aggiornata. Pertanto, con cui si desidera che i dati memorizzati nella cache del prodotto dipende non solo il `Products` tabella, ma nel `Categories` e `Suppliers` nonché tabelle.

Il [ `AggregateCacheDependency` classe](https://msdn.microsoft.com/library/system.web.caching.aggregatecachedependency.aspx) fornisce un mezzo per l'associazione di più dipendenze con un elemento della cache. Iniziare creando un `AggregateCacheDependency` istanza. Successivamente, aggiungere il set di dipendenze mediante il `AggregateCacheDependency` s `Add` metodo. Quando si inserisce l'elemento nella cache di dati in seguito, passare il `AggregateCacheDependency` istanza. Quando *qualsiasi* del `AggregateCacheDependency` istanza s dipendenze vengono modificate, l'elemento memorizzato nella cache verrà eliminato.

Di seguito è riportato il codice aggiornato per il `ProductsCL` classe s `AddCacheItem` metodo. Il metodo crea il `MasterCacheKeyArray` cache dipendenza insieme a `SqlCacheDependency` gli oggetti per il `Products`, `Categories`, e `Suppliers` tabelle. Queste sono combinate tutte in un unico `AggregateCacheDependency` oggetto denominato `aggregateDependencies`, che viene quindi passato il `Insert` metodo.


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample15.vb)]

Testare il nuovo codice di uscita. Viene visualizzata la `Products`, `Categories`, o `Suppliers` tabelle causano il rimozione dei dati memorizzati nella cache. Inoltre, il `ProductsCL` classe s `UpdateProduct` metodo, che viene chiamato durante la modifica di un prodotto tramite il controllo GridView, rimuove il `MasterCacheKeyArray` della dipendenza, provocando memorizzato nella cache dalla cache `ProductsDataTable` da rimuovere e i dati da recuperare nuovamente al successivo richiesta.

> [!NOTE]
> Dipendenze della cache SQL utilizzabili con [la memorizzazione nella cache di output](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/caching/output.aspx). Per una dimostrazione di questa funzionalità, vedere: [con cache di Output ASP.NET con SQL Server](https://msdn.microsoft.com/library/e3w8402y(VS.80).aspx).


## <a name="summary"></a>Riepilogo

Quando la memorizzazione nella cache dei dati del database, i dati in teoria rimangono nella cache fino a quando non viene modificata nel database. Con ASP.NET 2.0, le dipendenze della cache SQL possono essere create e utilizzate in scenari dichiarativi sia a livello di codice. Uno dei problemi con questo approccio è scoprire quando sono stati modificati i dati. Le versioni complete di Microsoft SQL Server 2005 forniscono le funzionalità di notifica che possono inviare un avviso un'applicazione quando viene modificato un risultato della query. Per la Express Edition di SQL Server 2005 e versioni precedenti di SQL Server, è invece necessario utilizzare un sistema di polling. Configurazione dell'infrastruttura necessarie polling Fortunatamente, è piuttosto semplice.

Buona programmazione!

## <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Utilizzando le notifiche delle Query in Microsoft SQL Server 2005](https://msdn.microsoft.com/library/ms175110.aspx)
- [Creazione di una notifica di Query](https://msdn.microsoft.com/library/ms188669.aspx)
- [La memorizzazione nella cache di ASP.NET con il `SqlCacheDependency` (classe)](https://msdn.microsoft.com/library/ms178604(VS.80).aspx)
- [Strumento di registrazione di ASP.NET SQL Server (`aspnet_regsql.exe`)](https://msdn.microsoft.com/library/ms229862(vs.80).aspx)
- [Panoramica di `SqlCacheDependency`](http://www.aspnetresources.com/blog/sql_cache_depedency_overview.aspx)

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Lead revisori per questa esercitazione sono stati Marko Rangel Teresa Murphy e Hilton Giesenow. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](caching-data-at-application-startup-vb.md)
