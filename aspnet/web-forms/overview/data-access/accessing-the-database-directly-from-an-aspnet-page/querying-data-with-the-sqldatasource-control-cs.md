---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-cs
title: Eseguire query sui dati con il controllo SqlDataSource (c#) | Microsoft Docs
author: rick-anderson
description: Nelle esercitazioni precedenti abbiamo utilizzato il controllo ObjectDataSource per separare completamente il livello di presentazione dal livello di accesso ai dati. A partire da questa tutor...
ms.author: riande
ms.date: 02/20/2007
ms.assetid: 60512d6a-b572-4b7a-beb3-3e44b4d2020c
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 0d15e09c2b790c4d1e6b278c4ea35bab7f66b861
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827648"
---
<a name="querying-data-with-the-sqldatasource-control-c"></a>Eseguire query sui dati con il controllo SqlDataSource (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'App di esempio](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_47_CS.exe) o [Scarica il PDF](querying-data-with-the-sqldatasource-control-cs/_static/datatutorial47cs1.pdf)

> Nelle esercitazioni precedenti abbiamo utilizzato il controllo ObjectDataSource per separare completamente il livello di presentazione dal livello di accesso ai dati. A partire da questa esercitazione, è informazioni su come utilizzare il controllo SqlDataSource per applicazioni semplici che non richiedono una netta separazione di presentazione e l'accesso ai dati.


## <a name="introduction"></a>Introduzione

Tutte le esercitazioni si va esaminato finora è stata utilizzata un'architettura a più livelli di presentazione, logica di Business e livelli di accesso ai dati. La Data Access Layer (DAL) è stato creato nella prima esercitazione ([creazione di un livello di accesso ai dati](../introduction/creating-a-data-access-layer-cs.md)) e il livello di logica di Business nella seconda ([creazione di un livello di logica di Business](../introduction/creating-a-business-logic-layer-cs.md)). Inizia con la [visualizzazione di dati con ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) dell'esercitazione, è stato illustrato come usare controllo ObjectDataSource di ASP.NET 2.0 s nuovo in modo dichiarativo interfacciarsi con l'architettura dal livello di presentazione.

Mentre tutte le esercitazioni finora hanno usato l'architettura per lavorare con i dati, è anche possibile accedere, inserire, aggiornare ed eliminare i dati del database direttamente da una pagina ASP.NET, ignorando l'architettura. Questa operazione inserisce le query di database specifico e la logica di business direttamente nella pagina web. Per le applicazioni sufficientemente ampio e complesse, progettazione, implementazione e usa un'architettura a più livelli è estremamente importante per l'esito positivo, degli aggiornamenti e la manutenibilità dell'applicazione. Sviluppa un'architettura solida, tuttavia, potrebbe non essere necessario quando si creano applicazioni estremamente semplici e singole.

ASP.NET 2.0 fornisce controlli origine dati incorporati cinque [SqlDataSource](https://msdn.microsoft.com/library/dz12d98w%28vs.80%29.aspx), [AccessDataSource](https://msdn.microsoft.com/library/8e5545e1.aspx), [ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx), [XmlDataSource](https://msdn.microsoft.com/library/e8d8587a%28en-US,VS.80%29.aspx), e [SiteMapDataSource](https://msdn.microsoft.com/library/5ex9t96x%28en-US,VS.80%29.aspx). SqlDataSource è utilizzabile per accedere e modificare i dati direttamente da un database relazionale, tra cui Microsoft SQL Server, Microsoft Access, Oracle, MySQL e altri utenti. In questa esercitazione e i successivi tre, verrà esaminato come utilizzare il controllo SqlDataSource, esplorazione come eseguire una query e filtrare i dati di database, nonché come utilizzare SqlDataSource per inserire, aggiornare ed eliminare dati.


![ASP.NET 2.0 include cinque controlli origine dati incorporata](querying-data-with-the-sqldatasource-control-cs/_static/image1.gif)

**Figura 1**: ASP.NET 2.0 include cinque controlli origine dati incorporata


## <a name="comparing-the-objectdatasource-and-sqldatasource"></a>Confronto tra la ObjectDataSource e SqlDataSource

Concettualmente, i controlli di ObjectDataSource e SqlDataSource sono semplicemente i proxy ai dati. Come descritto nel [visualizzazione di dati con ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) esercitazione ObjectDataSource ha proprietà che indicano il tipo di oggetto che fornisce i dati e i metodi da richiamare per selezionare, inserire, aggiornare ed eliminare dati il tipo di oggetto sottostante. Dopo aver configurate le proprietà di ObjectDataSource s, un controllo Web, ad esempio un controllo GridView, DetailsView o DataList dati possono essere associati al controllo, usando gli oggetti ObjectDataSource `Select()`, `Insert()`, `Delete()`, e `Update()` metodi interagire con l'architettura sottostante.

SqlDataSource fornisce la stessa funzionalità, ma viene eseguito su un database relazionale piuttosto che una libreria di oggetti. Con SqlDataSource, è necessario specificare la stringa di connessione di database e le query SQL ad hoc o le stored procedure da eseguire per inserire, aggiornare, eliminare e recuperare i dati. Le s SqlDataSource `Select()`, `Insert()`, `Update()`, e `Delete()` metodi, quando richiamata, connettersi al database specificato ed eseguire la query SQL appropriata. Come illustrato nel diagramma seguente, questi metodi eseguono il noioso compito di connessione a un database, esegue una query e restituzione dei risultati.


![SqlDataSource funge da Proxy per il Database](querying-data-with-the-sqldatasource-control-cs/_static/image2.gif)

**Figura 2**: SqlDataSource funge da Proxy per il Database


> [!NOTE]
> In questa esercitazione ci concentreremo sul recupero dei dati dal database. Nel [inserimento, aggiornamento ed eliminazione dei dati con il controllo SqlDataSource](inserting-updating-and-deleting-data-with-the-sqldatasource-cs.md) dell'esercitazione, si vedrà come configurare SqlDataSource per supportare l'inserimento, aggiornamento ed eliminazione.


## <a name="the-sqldatasource-and-accessdatasource-controls"></a>I controlli di AccessDataSource e SqlDataSource

Oltre al controllo SqlDataSource, ASP.NET 2.0 include anche un controllo AccessDataSource. Questi due diversi controlli causare molti sviluppatori familiarità con ASP.NET 2.0 sospettare che il controllo AccessDataSource è progettato per funzionare esclusivamente con Microsoft Access con il controllo SqlDataSource progettato per funzionare in modo esclusivo con Microsoft SQL Server. Sebbene il AccessDataSource è progettato per funzionare in modo specifico con Microsoft Access, il controllo SqlDataSource funziona con *qualsiasi* database relazionale in cui è possibile accedere tramite .NET. Ciò include qualsiasi archivi dati OLE DB o ODBC conformi a, ad esempio Microsoft SQL Server, Microsoft Access, Oracle, Informix, MySQL e PostgreSQL, tra le molte altre.

L'unica differenza tra i controlli SqlDataSource e AccessDataSource è come specificare le informazioni di connessione del database. Il controllo AccessDataSource richiede solo il percorso di file al file di database di Access. SqlDataSource, d'altra parte, richiede una stringa di connessione completa.

## <a name="step-1-creating-the-sqldatasource-web-pages"></a>Passaggio 1: Creazione di pagine Web SqlDataSource

Prima di iniziare a esplorare come lavorare direttamente con i dati di database usando il controllo SqlDataSource, consentire s prima di tutto si consiglia di creare le pagine ASP.NET nel progetto di sito Web che è necessario per questa esercitazione e i successivi tre. Iniziare aggiungendo una nuova cartella denominata `SqlDataSource`. Successivamente, aggiungere le seguenti pagine ASP.NET per quella cartella, assicurandosi di associare ogni pagina con il `Site.master` pagina master:

- `Default.aspx`
- `Querying.aspx`
- `ParameterizedQueries.aspx`
- `InsertUpdateDelete.aspx`
- `OptimisticConcurrency.aspx`


![Aggiungere le pagine ASP.NET per le esercitazioni relative alla SqlDataSource](querying-data-with-the-sqldatasource-control-cs/_static/image3.gif)

**Figura 3**: aggiungere le pagine ASP.NET per le esercitazioni relative alla SqlDataSource


In altre cartelle, analogo a `Default.aspx` nella `SqlDataSource` cartella elencherà le esercitazioni nella relativa sezione. Si tenga presente che il `SectionLevelTutorialListing.ascx` controllo utente fornisce questa funzionalità. Pertanto, aggiungere questo controllo utente da `Default.aspx` trascinandolo da Esplora soluzioni nella pagina di visualizzazione della struttura s.


[![Aggiungere il controllo utente sectionleveltutoriallisting. ascx a default. aspx](querying-data-with-the-sqldatasource-control-cs/_static/image5.gif)](querying-data-with-the-sqldatasource-control-cs/_static/image4.gif)

**Figura 4**: aggiungere i `SectionLevelTutorialListing.ascx` controllo utente da `Default.aspx` ([fare clic per visualizzare l'immagine con dimensioni normali](querying-data-with-the-sqldatasource-control-cs/_static/image6.gif))


Infine, aggiungere questi quattro pagine come voci per il `Web.sitemap` file. In particolare, aggiungere il markup seguente dopo l'aggiunta di pulsanti personalizzati con DataList e Repeater `<siteMapNode>`:


[!code-sql[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample1.sql)]

Dopo aver aggiornato `Web.sitemap`, si consiglia di visualizzare il sito Web di esercitazioni tramite un browser. Il menu a sinistra ora include elementi per la modifica, inserimento ed eliminazione di esercitazioni.


![Mappa del sito include ora voci per le esercitazioni SqlDataSource](querying-data-with-the-sqldatasource-control-cs/_static/image7.gif)

**Figura 5**: la mappa del sito include ora voci per le esercitazioni SqlDataSource


## <a name="step-2-adding-and-configuring-the-sqldatasource-control"></a>Passaggio 2: Aggiunta e configurazione del controllo SqlDataSource

Iniziare aprendo il `Querying.aspx` nella pagina di `SqlDataSource` cartella e passare alla visualizzazione progettazione. Trascinare un controllo SqlDataSource dalla casella degli strumenti nella finestra di progettazione e set relativo `ID` a `ProductsDataSource`. Come con ObjectDataSource, SqlDataSource non genera alcun output sottoposto a rendering e pertanto viene visualizzato come un riquadro grigio nell'area di progettazione. Per configurare SqlDataSource, fare clic sul collegamento Configura origine dati nello smart tag SqlDataSource s.


![Scegliere il collegamento Configura origine dati nello Smart tag s SqlDataSource](querying-data-with-the-sqldatasource-control-cs/_static/image8.gif)

**Figura 6**: scegliere il collegamento Configura origine dati SqlDataSource s nello Smart tag


Verrà visualizzata la procedura guidata Configura origine dati di s controllo SqlDataSource. Durante la procedura guidata s diversa dal controllo ObjectDataSource s, l'obiettivo finale è lo stesso per fornire i dettagli su come recuperare, inserire, aggiornare ed eliminare dati tramite l'origine dati. Per SqlDataSource questo comporta che specifica il database sottostante da utilizzare e che forniscono le istruzioni SQL ad hoc o le stored procedure.

Il primo passaggio della procedura guidata richiede il database. L'elenco di riepilogo a discesa include tali database trovati in s dell'applicazione web `App_Data` cartella e quelli che sono state aggiunte al nodo Connessioni dati in Esplora Server. Poiché è stato aggiunto già una stringa di connessione per il `NORTHWIND.MDF` del database nel `App_Data` cartella al nostro progetto s `Web.config` file, nell'elenco a discesa include un riferimento a tale stringa di connessione, `NORTHWINDConnectionString`. Scegliere questa voce dall'elenco a discesa elenco e fare clic su Avanti.


![Scegliere il NORTHWINDConnectionString dall'elenco a discesa](querying-data-with-the-sqldatasource-control-cs/_static/image9.gif)

**Figura 7**: scegliere la `NORTHWINDConnectionString` nell'elenco a discesa


Dopo aver scelto il database, la procedura guidata richiede la query restituire i dati. È possibile specificare le colonne di una tabella o vista per restituire può immettere un'istruzione SQL personalizzata o specificare una stored procedure. È possibile alternare questa scelta tramite la specifica un'istruzione SQL personalizzata o stored procedure e specificare le colonne da una tabella o visualizzare i pulsanti di opzione.

> [!NOTE]
> Per questo primo esempio, consentire s utilizzare il specificare le colonne di un'opzione di tabella o vista. Verrà restituito alla procedura guidata più avanti in questa esercitazione ed esplorare specifica un'opzione della stored procedure o un'istruzione SQL personalizzata.


Figura 8 Mostra configura la schermata di istruzione Select quando è selezionata la specificare le colonne di un pulsante di opzione di tabella o vista. L'elenco a discesa contiene il set di tabelle e viste nel database Northwind, con la tabella selezionata o colonne della vista s visualizzate nell'elenco casella di controllo seguente. Per questo esempio, restituire let s il `ProductID`, `ProductName`, e `UnitPrice` le colonne dai `Products` tabella. Come illustrato nella figura 8, dopo la procedura guidata queste selezioni Mostra l'istruzione SQL risultante `SELECT [ProductID], [ProductName], [UnitPrice] FROM [Products]`.


![Restituire i dati dalla tabella Products](querying-data-with-the-sqldatasource-control-cs/_static/image10.gif)

**Figura 8**: dati restituiti dai `Products` tabella


Dopo aver configurato la procedura guidata per restituire il `ProductID`, `ProductName`, e `UnitPrice` le colonne dai `Products` , fare clic sul pulsante Avanti. Questa schermata finale fornisce un'opportunità per esaminare i risultati della query configurato nel passaggio precedente. Fare clic sul pulsante Query di Test viene eseguita l'applicazione configurata `SELECT` istruzione e consente di visualizzare i risultati in una griglia.


![Fare clic sul pulsante Query di prova per rivedere la Query SELECT](querying-data-with-the-sqldatasource-control-cs/_static/image11.gif)

**Figura 9**: fare clic sul pulsante Query di Test per la revisione di `SELECT` Query


Per completare la procedura guidata, fare clic su Fine.

Come con ObjectDataSource, la procedura guidata s SqlDataSource si limita ad assegna valori alle proprietà del controllo del codice s, vale a dire il [ `ConnectionString` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.connectionstring.aspx) e [ `SelectCommand` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.selectcommand.aspx) proprietà. Dopo aver completato la procedura guidata, il markup dichiarativo del controllo s SqlDataSource dovrebbe essere simile al seguente:


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample2.aspx)]

Il `ConnectionString` proprietà fornisce informazioni su come connettersi al database. Questa proprietà può essere assegnata un valore di stringa di connessione completa a livello di codice oppure può puntare a una stringa di connessione in `Web.config`. Per fare riferimento a un valore di stringa di connessione nel file Web. config, usare la sintassi `<%$ expressionPrefix:expressionValue %>`. In genere *expressionPrefix* è ConnectionStrings e *expressionValue* è il nome della stringa di connessione nel `Web.config` [ `<connectionStrings>` sezione](https://msdn.microsoft.com/library/bf7sd233.aspx). Tuttavia, è possibile usare la sintassi per fare riferimento a `<appSettings>` elementi o il contenuto dai file di risorse. Visualizzare [Panoramica di espressioni ASP.NET](https://msdn.microsoft.com/library/d5bd1tad.aspx) per altre informazioni su questa sintassi.

Il `SelectCommand` proprietà consente di specificare l'istruzione SQL ad hoc o stored procedure da eseguire per restituire i dati.

## <a name="step-3-adding-a-data-web-control-and-binding-it-to-the-sqldatasource"></a>Passaggio 3: Aggiunta di un controllo Web e associarla a SqlDataSource

Dopo aver configurato il SqlDataSource, può essere associato a un controllo Web, ad esempio un controllo GridView o DetailsView di dati. Per questa esercitazione, lasciare s visualizzare i dati in un controllo GridView. Dalla casella degli strumenti, trascinare un controllo GridView sulla pagina e associarlo al `ProductsDataSource` SqlDataSource scegliendo l'origine dati nell'elenco a discesa nello smart tag s GridView.


[![Aggiungere un controllo GridView e associarlo al controllo SqlDataSource](querying-data-with-the-sqldatasource-control-cs/_static/image13.gif)](querying-data-with-the-sqldatasource-control-cs/_static/image12.gif)

**Figura 10**: aggiungere un controllo GridView e associarlo al controllo SqlDataSource ([fare clic per visualizzare l'immagine con dimensioni normali](querying-data-with-the-sqldatasource-control-cs/_static/image14.gif))


Dopo che è già selezionato il controllo SqlDataSource dall'elenco a discesa nello smart tag s GridView, Visual Studio aggiungerà automaticamente un BoundField o CampoCasellaDiControllo a GridView per ognuna delle colonne restituite dal controllo origine dati. Poiché SqlDataSource restituisce tre colonne di database `ProductID`, `ProductName`, e `UnitPrice` nel GridView sono presenti tre campi.

Si consiglia di configurare i dispositivi di GridView tre BoundField. Modifica il `ProductName` il campo s `HeaderText` proprietà per nome prodotto e `UnitPrice` il campo s al prezzo. Anche formattare i `UnitPrice` campo come valuta. Dopo aver apportato queste modifiche, il markup dichiarativo di GridView s dovrebbe essere simile al seguente:


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample3.aspx)]

Visitare questa pagina tramite un browser. Come illustrato nella figura 11, il controllo GridView Elenca ogni prodotto 1!s `ProductID`, `ProductName`, e `UnitPrice` valori.


[![GridView Visualizza ogni prodotto s ProductID, ProductName e valori UnitPrice](querying-data-with-the-sqldatasource-control-cs/_static/image16.gif)](querying-data-with-the-sqldatasource-control-cs/_static/image15.gif)

**Figura 11**: il controllo GridView Visualizza ogni prodotto s `ProductID`, `ProductName`, e `UnitPrice` valori ([fare clic per visualizzare l'immagine con dimensioni normali](querying-data-with-the-sqldatasource-control-cs/_static/image17.gif))


Quando si visita la pagina di GridView richiama il controllo del codice sorgente dati s `Select()` (metodo). Quando si stava usando il controllo ObjectDataSource, questa chiamata il `ProductsBLL` classe s `GetProducts()` (metodo). Con SqlDataSource, tuttavia, il `Select()` metodo stabilisce una connessione al database specificato e i problemi il `SelectCommand` (`SELECT [ProductID], [ProductName], [UnitPrice] FROM [Products]`, in questo esempio). SqlDataSource restituisce i risultati che il controllo GridView viene enumerato, creazione di una riga in GridView per ogni record di database restituiti.

## <a name="the-built-in-data-web-control-features-and-the-sqldatasource-control"></a>Le funzionalità di controllo Web di dati incorporato e il controllo SqlDataSource

In generale, le funzionalità intrinseche per i dati di paging, ordinamento, modifica, i controlli Web l'eliminazione, inserimento e così via sono specifiche per il controllo Web per dati e non dipendono dal controllo origine dati usato. Vale a dire, il controllo GridView può utilizzare relativo predefinito di paging, ordinamento, modifica ed eliminazione se è associato a ObjectDataSource o un SqlDataSource. Tuttavia, determinati funzionalità di controllo Web i dati sono riservati per il controllo origine dati in uso o per la configurazione di controllo del codice s dell'origine dati.

Ad esempio, nel [in modo efficiente il Paging attraverso quantità di dati di grandi dimensioni](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs.md) esercitazione è stata illustrata la procedura, per impostazione predefinita, la logica di paging per i dati Web controlla gestire restituisce *tutti* i record da sottostante origine dati e quindi visualizzano solo il subset appropriato di record specificato l'indice della pagina corrente e il numero di record da visualizzare per pagina. Questo modello è particolarmente inefficiente quando paging dei set di risultati sufficientemente grandi. Fortunatamente, ObjectDataSource può essere configurati per supportare il paging personalizzato, che restituisce solo il subset preciso di record da visualizzare. Il controllo SqlDataSource, tuttavia, non ha le proprietà per implementare il paging personalizzato.

Si verifica un altro accorgimento con paging e ordinamento con SqlDataSource. Per impostazione predefinita, i dati restituiti da un SqlDataSource possono essere di paging o ordinati tramite il controllo GridView. Per dimostrare questo concetto, controllare le opzioni di abilitare il Paging e abilitare l'ordinamento in GridView s smart tag in `Querying.aspx` e verificare che funzioni come previsto.

Ordinamento e paging funziona perché SqlDataSource recupera i dati di database in un set di dati non fortemente tipizzato. Che è possibile ottenere il numero totale di record restituiti dalla query un aspetto essenziale per implementare il paging del set di dati. Inoltre, i risultati di s set di dati possono essere ordinati tramite un oggetto DataView. Queste funzionalità vengono usate da SqlDataSource automaticamente quando le richieste di GridView di paging o i dati ordinati.

SqlDataSource può essere configurato per restituire un oggetto DataReader invece di un set di dati tramite la modifica relativa [ `DataSourceMode` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.datasourcemode.aspx) dalla `DataSet` (predefinito) a `DataReader`. Potrebbe essere preferito tramite un oggetto DataReader in situazioni quando si passano i risultati di s SqlDataSource al codice esistente che si aspetta un DataReader. Inoltre, poiché DataReader sono oggetti notevolmente più semplici rispetto a set di dati, queste connessioni offrono prestazioni migliori. Se si apporta questa modifica, tuttavia, non è possibile ordinare il controllo Web per dati né pagina poiché SqlDataSource non è possibile verificare il numero di record viene restituito dalla query e neppure DataReader offrono tutte le tecniche per l'ordinamento dei dati restituiti.

## <a name="step-4-using-a-custom-sql-statement-or-stored-procedure"></a>Passaggio 4: Uso di un'istruzione SQL personalizzata o una Stored Procedure

Quando si configura il controllo SqlDataSource, la query utilizzata per restituire i dati può essere specificata in uno dei due approcci come un'istruzione SQL personalizzata o una stored procedure o come colonne di una tabella o vista esistente. Nel passaggio 2 sono stati esaminati selezionando le colonne dai `Products` tabella. Let s spiega come usare un'istruzione SQL personalizzata.

Aggiungere un altro controllo GridView di `Querying.aspx` pagina e scegliere di creare una nuova origine dati dall'elenco a discesa scegliere nello smart tag. Successivamente, indicare che i dati verranno prelevati da un database si creerà un nuovo controllo SqlDataSource. Denominare il controllo `ProductsWithCategoryInfoDataSource`.


![Creare un nuovo controllo SqlDataSource denominato ProductsWithCategoryInfoDataSource](querying-data-with-the-sqldatasource-control-cs/_static/image18.gif)

**Figura 12**: creare un nuovo controllo SqlDataSource denominato `ProductsWithCategoryInfoDataSource`


Nella schermata successiva viene chiesto di specificare per specificare il database. Come è stato fatto nella figura 7, selezionare il `NORTHWINDConnectionString` dall'elenco a discesa elenco e fare clic su Avanti. Schermata di istruzione Select della configurazione, scegliere di specificare un pulsante di opzione di stored procedure o un'istruzione SQL personalizzata e fare clic su Avanti. Verrà visualizzata la schermata di Stored procedure o definire istruzioni personalizzate, che offre schede: SELECT, UPDATE, INSERT e DELETE. In ogni scheda è possibile immettere un'istruzione SQL personalizzata nella casella di testo o scegliere una stored procedure dall'elenco a discesa. In questa esercitazione si esaminerà immettendo un'istruzione SQL personalizzata. l'esercitazione successiva include un esempio che usa una stored procedure.


![Immettere un'istruzione SQL personalizzata o una Stored Procedure di selezione](querying-data-with-the-sqldatasource-control-cs/_static/image19.gif)

**Figura 13**: immettere un'istruzione SQL personalizzata o una Stored Procedure di selezione


L'istruzione SQL personalizzata può essere immessi manualmente nella casella di testo o può essere costruito graficamente facendo clic sul pulsante Generatore delle Query. Dal generatore di Query o la casella di testo, utilizzare la query seguente per restituire il `ProductID` e `ProductName` i campi dalle `Products` tabella usando una `JOIN` per recuperare il prodotto s `CategoryName` dal `Categories` tabella:


[!code-sql[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample4.sql)]


![È possibile graficamente costruire la Query utilizzando il generatore delle Query](querying-data-with-the-sqldatasource-control-cs/_static/image20.gif)

**Figura 14**: È possibile creare graficamente la Query utilizzando il generatore delle Query


Dopo aver specificato la query, fare clic su Avanti per passare alla schermata di Query di Test. Fare clic su Fine per completare la procedura guidata SqlDataSource.

Dopo aver completato la procedura guidata, il controllo GridView avrà tre BoundField aggiunto a tale visualizzazione il `ProductID`, `ProductName`, e `CategoryName` colonne restituite dalla query e risultante nel markup dichiarativo seguenti:


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample5.aspx)]


[![GridView Visualizza ogni ID prodotto s, nome della categoria di nome e associato](querying-data-with-the-sqldatasource-control-cs/_static/image22.gif)](querying-data-with-the-sqldatasource-control-cs/_static/image21.gif)

**Figura 15**: il controllo GridView Mostra ogni prodotto s ID, Name e associate nome della categoria ([fare clic per visualizzare l'immagine con dimensioni normali](querying-data-with-the-sqldatasource-control-cs/_static/image23.gif))


## <a name="summary"></a>Riepilogo

In questa esercitazione è stato illustrato come eseguire query e visualizzare i dati usando il controllo SqlDataSource. Ad esempio ObjectDataSource, SqlDataSource funge da proxy, che fornisce un approccio dichiarativo per l'accesso ai dati. Le relative proprietà specificano il database a cui connettersi e il codice SQL `SELECT` la query da eseguire; possono essere specificate tramite la finestra proprietà o tramite la procedura guidata Configura origine dati.

Il `SELECT` esempi di query viene esaminati in questa esercitazione tutti i record restituiscano dalla query specificata. Tuttavia, il controllo SqlDataSource, può includere un `WHERE` clausola con parametri i cui valori vengono assegnati a livello di codice o vengono automaticamente eseguito il pull da un'origine specificata. Come creare e usare query con parametri nella prossima esercitazione verrà esaminato.

Buona programmazione!

## <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [L'accesso ai dati di Database relazionale](http://aspnet.4guysfromrolla.com/articles/022206-1.aspx)
- [Cenni preliminari sul controllo SqlDataSource](https://msdn.microsoft.com/library/dz12d98w.aspx)
- [Esercitazioni della Guida rapida ASP.NET: Il controllo SqlDataSource](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/data/sqldatasource.aspx)
- [Il file Web. config `<connectionStrings>` elemento](https://msdn.microsoft.com/library/bf7sd233.aspx)
- [Riferimento alla stringa di connessione di database](http://www.connectionstrings.com/)

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. I revisori per questa esercitazione sono state Connery Susan, Bernadette Leigh e David Suru. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [avanti](using-parameterized-queries-with-the-sqldatasource-cs.md)
