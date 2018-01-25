---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-cs
title: Eseguire query sui dati con il controllo SqlDataSource (c#) | Documenti Microsoft
author: rick-anderson
description: Nelle esercitazioni precedenti abbiamo utilizzato il controllo ObjectDataSource per separare completamente il livello di presentazione dal livello di accesso ai dati. A partire da questa tutor...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2007
ms.topic: article
ms.assetid: 60512d6a-b572-4b7a-beb3-3e44b4d2020c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 4652e5820e621a7b2ad3b03bb5a1d2cb4968fadd
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="querying-data-with-the-sqldatasource-control-c"></a>Eseguire query sui dati con il controllo SqlDataSource (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare App di esempio](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_47_CS.exe) o [Scarica il PDF](querying-data-with-the-sqldatasource-control-cs/_static/datatutorial47cs1.pdf)

> Nelle esercitazioni precedenti abbiamo utilizzato il controllo ObjectDataSource per separare completamente il livello di presentazione dal livello di accesso ai dati. A partire da questa esercitazione, è illustrato come utilizzare il controllo SqlDataSource per applicazioni semplici che non richiedono una separazione tali strict di presentazione e l'accesso ai dati.


## <a name="introduction"></a>Introduzione

Tutte le esercitazioni si ve esaminata finora è stata utilizzata un'architettura a più livelli composta da presentazione, logica di Business e livelli di accesso ai dati. Il livello accesso dati (DAL) è stato predisposto nella prima esercitazione ([la creazione di un livello di accesso ai dati](../introduction/creating-a-data-access-layer-cs.md)) e il livello di logica di Business al secondo ([la creazione di un livello di logica di Business](../introduction/creating-a-business-logic-layer-cs.md)). A partire dal [la visualizzazione di dati con ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) dell'esercitazione, è stato illustrato come utilizzare il controllo ObjectDataSource nuovo di ASP.NET 2.0 s per l'interfaccia in modo dichiarativo con l'architettura dal livello di presentazione.

Mentre tutte le esercitazioni finora utilizzato l'architettura per lavorare con dati, è anche possibile accedere, inserire, aggiornare ed eliminare dati di database direttamente da una pagina ASP.NET, ignorando l'architettura. Questa operazione inserisce le query di database specifico e una logica di business direttamente nella pagina web. Per le applicazioni sufficientemente ampio e complesse, progettazione, l'implementazione e utilizzo di un'architettura a più livelli è di vitale importanza per l'esito positivo, l'aggiornabilità e manutenibilità dell'applicazione. Lo sviluppo di una solida architettura, tuttavia, potrebbe non essere necessario quando si creano applicazioni estremamente semplice, One-Off.

ASP.NET 2.0 fornisce controlli origine dati incorporati cinque [SqlDataSource](https://msdn.microsoft.com/library/dz12d98w%28vs.80%29.aspx), [AccessDataSource](https://msdn.microsoft.com/library/8e5545e1.aspx), [ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx), [XmlDataSource](https://msdn.microsoft.com/library/e8d8587a%28en-US,VS.80%29.aspx), e [SiteMapDataSource](https://msdn.microsoft.com/library/5ex9t96x%28en-US,VS.80%29.aspx). SqlDataSource consente di accedere e modificare i dati direttamente da un database relazionale, incluso Microsoft SQL Server, Microsoft Access, Oracle, MySQL e ad altri utenti. In questa esercitazione e i successivi tre, verrà esaminato come utilizzare il controllo SqlDataSource, esplorazione come eseguire una query e filtrare i dati di database, nonché come utilizzare SqlDataSource per inserire, aggiornare ed eliminare dati.


![ASP.NET 2.0 include cinque controlli origine dati incorporata](querying-data-with-the-sqldatasource-control-cs/_static/image1.gif)

**Figura 1**: ASP.NET 2.0 include cinque controlli origine dati incorporata


## <a name="comparing-the-objectdatasource-and-sqldatasource"></a>Confronto tra le ObjectDataSource e SqlDataSource

Concettualmente, controlli ObjectDataSource sia SqlDataSource sono semplicemente i proxy ai dati. Come descritto nel [la visualizzazione di dati con ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) esercitazione ObjectDataSource ha proprietà che indicano il tipo di oggetto che fornisce i dati e i metodi da richiamare per selezionare, inserire, aggiornare ed eliminare dati il tipo di oggetto sottostante. Dopo aver configurate le proprietà di ObjectDataSource s, possono essere associati un controllo Web, ad esempio un GridView, DetailsView o DataList dati al controllo, con i dispositivi ObjectDataSource `Select()`, `Insert()`, `Delete()`, e `Update()` metodi interagire con l'architettura sottostante.

SqlDataSource fornisce la stessa funzionalità, ma viene eseguito su un database relazionale piuttosto che una libreria di oggetti. Con SqlDataSource, è necessario specificare la stringa di connessione di database e le query SQL ad hoc o le stored procedure da eseguire per inserire, aggiornare, eliminare e recuperare i dati. S SqlDataSource `Select()`, `Insert()`, `Update()`, e `Delete()` metodi, quando richiamata, connettersi al database specificato ed eseguire la query SQL appropriata. Come illustrato nel diagramma seguente, questi metodi eseguono le operazioni di grunt di connessione a un database, l'esecuzione di una query e restituzione dei risultati.


![SqlDataSource funge da Proxy per il Database](querying-data-with-the-sqldatasource-control-cs/_static/image2.gif)

**Figura 2**: SqlDataSource funge da Proxy per il Database


> [!NOTE]
> In questa esercitazione verrà descritto come il recupero dei dati dal database. Nel [inserimento, aggiornamento e l'eliminazione di dati con il controllo SqlDataSource](inserting-updating-and-deleting-data-with-the-sqldatasource-cs.md) dell'esercitazione, si vedrà come configurare SqlDataSource per supportare l'inserimento, aggiornamento ed eliminazione.


## <a name="the-sqldatasource-and-accessdatasource-controls"></a>I controlli di AccessDataSource e di SqlDataSource

Oltre al controllo SqlDataSource, ASP.NET 2.0 include anche un controllo AccessDataSource. Questi due diversi controlli causare molti sviluppatori di nuovo a ASP.NET 2.0 sospettare che il controllo AccessDataSource è progettato per funzionare in modo esclusivo con Microsoft Access con il controllo SqlDataSource progettato per funzionare in modo esclusivo con Microsoft SQL Server. Mentre l'AccessDataSource è progettato per funzionare in modo specifico con Microsoft Access, il controllo SqlDataSource funziona con *qualsiasi* database relazionale in cui è possibile accedere tramite .NET. Ciò include qualsiasi archivi dati OLE DB o ODBC conforme a, ad esempio Microsoft SQL Server, Microsoft Access, Oracle, Informix, MySQL e PostgreSQL, tra molti altri.

L'unica differenza tra i controlli AccessDataSource e SqlDataSource è come specificare le informazioni di connessione del database. Il controllo AccessDataSource richiede solo il percorso del file al file di database di Access. SqlDataSource, d'altra parte, richiede una stringa di connessione completa.

## <a name="step-1-creating-the-sqldatasource-web-pages"></a>Passaggio 1: Creazione delle pagine Web SqlDataSource

Prima di iniziare l'esplorazione di come utilizzare direttamente i dati del database utilizzando il controllo SqlDataSource, consentire s attentamente prima di creare le pagine ASP.NET nel progetto sito Web che è necessario per questa esercitazione e i successivi tre. Per iniziare, aggiungere una nuova cartella denominata `SqlDataSource`. Successivamente, aggiungere le seguenti pagine ASP.NET a quella cartella, assicurandosi di associare ogni pagina con il `Site.master` pagina master:

- `Default.aspx`
- `Querying.aspx`
- `ParameterizedQueries.aspx`
- `InsertUpdateDelete.aspx`
- `OptimisticConcurrency.aspx`


![Aggiungere le pagine ASP.NET per le esercitazioni relative SqlDataSource](querying-data-with-the-sqldatasource-control-cs/_static/image3.gif)

**Figura 3**: aggiungere le pagine ASP.NET per le esercitazioni relative SqlDataSource


Come in altre cartelle, `Default.aspx` nel `SqlDataSource` cartella elencherà le esercitazioni nella relativa sezione. Tenere presente che il `SectionLevelTutorialListing.ascx` controllo utente fornisce questa funzionalità. Pertanto, aggiungere il controllo utente `Default.aspx` trascinandolo da Esplora soluzioni nella pagina di visualizzazione della struttura s.


[![Aggiungere il controllo utente SectionLevelTutorialListing.ascx Default.aspx](querying-data-with-the-sqldatasource-control-cs/_static/image5.gif)](querying-data-with-the-sqldatasource-control-cs/_static/image4.gif)

**Figura 4**: aggiungere il `SectionLevelTutorialListing.ascx` controllo utente in `Default.aspx` ([fare clic per visualizzare l'immagine ingrandita](querying-data-with-the-sqldatasource-control-cs/_static/image6.gif))


Infine, aggiungere questi quattro pagine come voci per il `Web.sitemap` file. In particolare, aggiungere il markup seguente dopo l'aggiunta di pulsanti personalizzata al DataList e Repeater `<siteMapNode>`:


[!code-sql[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample1.sql)]

Dopo aver aggiornato `Web.sitemap`, dedicare alcuni minuti per visualizzare il sito Web esercitazioni tramite un browser. Il menu a sinistra include elementi per la modifica, inserimento ed eliminazione di esercitazioni.


![Mappa del sito include ora le voci per le esercitazioni SqlDataSource](querying-data-with-the-sqldatasource-control-cs/_static/image7.gif)

**Figura 5**: mappa del sito include ora le voci per le esercitazioni SqlDataSource


## <a name="step-2-adding-and-configuring-the-sqldatasource-control"></a>Passaggio 2: Aggiunta e configurazione del controllo SqlDataSource

Aprire il `Querying.aspx` nella pagina di `SqlDataSource` cartella e passare alla visualizzazione progettazione. Trascinare un controllo SqlDataSource dalla casella degli strumenti di progettazione e il set relativo `ID` a `ProductsDataSource`. Come con ObjectDataSource, SqlDataSource non genera alcun output sottoposto a rendering e di conseguenza, viene visualizzato come casella grigia nell'area di progettazione. Per configurare SqlDataSource, fare clic sul collegamento Configura origine dati da smart tag SqlDataSource s.


![Scegliere il collegamento Configura origine dati da SqlDataSource s Smart Tag](querying-data-with-the-sqldatasource-control-cs/_static/image8.gif)

**Figura 6**: scegliere il collegamento Configura origine dati da SqlDataSource s Smart Tag


Verrà visualizzata la procedura guidata Configura origine dati di s controllo SqlDataSource. Durante la procedura guidata s differisce dal controllo ObjectDataSource s, l'obiettivo finale è lo stesso per fornire i dettagli su come recuperare, inserire, aggiornare ed eliminare dati tramite l'origine dati. Per SqlDataSource questo implica che specifica il database sottostante da utilizzare e fornire le istruzioni SQL ad hoc o le stored procedure.

Il primo passaggio della procedura guidata richiede Microsoft per il database. L'elenco di riepilogo a discesa include i database nell'applicazione web s `App_Data` cartella e quelli che sono stati aggiunti al nodo Connessioni di dati in Esplora Server. Poiché è già aggiunta una stringa di connessione per il `NORTHWIND.MDF` database il `App_Data` cartella al progetto s `Web.config` file, nell'elenco a discesa include un riferimento alla stessa stringa di connessione, `NORTHWINDConnectionString`. Scegliere questa voce dall'elenco a discesa e fare clic su Avanti.


![Scegliere il NORTHWINDConnectionString dall'elenco a discesa](querying-data-with-the-sqldatasource-control-cs/_static/image9.gif)

**Figura 7**: scegliere il `NORTHWINDConnectionString` dall'elenco a discesa


Dopo aver scelto il database, la procedura guidata richiede la query restituire i dati. È possibile specificare le colonne di una tabella o vista per restituire può immettere un'istruzione SQL personalizzata o specificare una stored procedure. È possibile passare tra questa scelta tramite una specifica istruzione SQL o stored procedure e specificare le colonne di una tabella o visualizzare pulsanti di opzione.

> [!NOTE]
> Per questo primo esempio, consentire s utilizzare di specificare le colonne di un'opzione di tabella o vista. Viene restituito alla procedura guidata, più avanti in questa esercitazione ed esplorare specificare un'opzione stored procedure o un'istruzione SQL personalizzata.


Figura 8 è illustrata la configurazione la schermata di istruzione Select quando è selezionata la specificare le colonne di un pulsante di opzione di tabella o vista. L'elenco a discesa contiene il set di tabelle e viste nel database Northwind, con la tabella selezionata o le colonne della vista s visualizzate nell'elenco casella di controllo. Per questo esempio, s consentono di restituire il `ProductID`, `ProductName`, e `UnitPrice` le colonne di `Products` tabella. Come illustrato nella figura 8, dopo la procedura guidata queste selezioni Mostra l'istruzione SQL risultante `SELECT [ProductID], [ProductName], [UnitPrice] FROM [Products]`.


![Restituire i dati dalla tabella Products](querying-data-with-the-sqldatasource-control-cs/_static/image10.gif)

**Figura 8**: dati restituiti dal `Products` tabella


Dopo aver configurato la procedura guidata per restituire il `ProductID`, `ProductName`, e `UnitPrice` le colonne di `Products` , fare clic sul pulsante Avanti. Questa schermata finale offre l'opportunità per esaminare i risultati della query configurato nel passaggio precedente. Fare clic sul pulsante Test Query esegue l'applicazione configurata `SELECT` istruzione e visualizza i risultati in una griglia.


![Fare clic sul pulsante Query di Test per esaminare la Query di selezione](querying-data-with-the-sqldatasource-control-cs/_static/image11.gif)

**Figura 9**: fare clic sul pulsante Query di Test per esaminare il `SELECT` Query


Per completare la procedura guidata, fare clic su Fine.

Come con ObjectDataSource, la procedura guidata s SqlDataSource si limita ad assegna valori alle proprietà del controllo s, vale a dire il [ `ConnectionString` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.connectionstring.aspx) e [ `SelectCommand` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.selectcommand.aspx) proprietà. Dopo aver completato la procedura guidata, markup dichiarativo s controllo SqlDataSource dovrebbe essere simile al seguente:


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample2.aspx)]

Il `ConnectionString` proprietà fornisce informazioni su come connettersi al database. Questa proprietà può essere assegnata un valore di stringa di connessione completa a livello di codice oppure può puntare a una stringa di connessione in `Web.config`. Per fare riferimento a un valore di stringa di connessione in Web. config, usare la sintassi `<%$ expressionPrefix:expressionValue %>`. In genere, *expressionPrefix* è ConnectionStrings e *expressionValue* è il nome della stringa di connessione nel `Web.config` [ `<connectionStrings>` sezione](https://msdn.microsoft.com/library/bf7sd233.aspx). Tuttavia, è possibile utilizzare la sintassi per riferimento `<appSettings>` elementi o il contenuto dai file di risorse. Vedere [Cenni preliminari sulle espressioni ASP.NET](https://msdn.microsoft.com/library/d5bd1tad.aspx) per ulteriori informazioni su questa sintassi.

Il `SelectCommand` proprietà specifica la stored procedure da eseguire per restituire i dati o l'istruzione SQL ad hoc.

## <a name="step-3-adding-a-data-web-control-and-binding-it-to-the-sqldatasource"></a>Passaggio 3: Aggiunta di un controllo Web di dati e associarlo a SqlDataSource

Una volta SqlDataSource è stato configurato, può essere associato a un controllo Web, ad esempio un controllo GridView o DetailsView di dati. Per questa esercitazione, lasciare s visualizzare i dati in un controllo GridView. Dalla casella degli strumenti, trascinare un controllo GridView nella pagina e associarlo al `ProductsDataSource` SqlDataSource scegliendo l'origine dati dall'elenco a discesa nello smart tag GridView s.


[![Aggiungere un controllo GridView e associarlo al controllo SqlDataSource](querying-data-with-the-sqldatasource-control-cs/_static/image13.gif)](querying-data-with-the-sqldatasource-control-cs/_static/image12.gif)

**Figura 10**: aggiungere un controllo GridView e associarlo al controllo SqlDataSource ([fare clic per visualizzare l'immagine ingrandita](querying-data-with-the-sqldatasource-control-cs/_static/image14.gif))


Una volta è già selezionato il controllo SqlDataSource dall'elenco a discesa nello smart tag GridView s, Visual Studio aggiungerà automaticamente un BoundField o CheckBoxField a GridView per ognuna delle colonne restituiti dal controllo origine dati. Poiché SqlDataSource restituisce tre colonne di database `ProductID`, `ProductName`, e `UnitPrice` esistono tre campi in GridView.

È opportuno configurare i dispositivi di GridView tre BoundField. Modifica il `ProductName` il campo s `HeaderText` proprietà per nome prodotto e `UnitPrice` il campo s al prezzo. È inoltre di formattare il `UnitPrice` campo come valuta. Dopo aver apportato queste modifiche, il markup dichiarativo di GridView s dovrebbe essere simile al seguente:


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample3.aspx)]

Visitare questa pagina tramite un browser. Come mostrato nella figura 11, GridView Elenca ogni prodotto s `ProductID`, `ProductName`, e `UnitPrice` valori.


[![GridView Visualizza ogni prodotto s ProductID, ProductName e i valori di UnitPrice](querying-data-with-the-sqldatasource-control-cs/_static/image16.gif)](querying-data-with-the-sqldatasource-control-cs/_static/image15.gif)

**Figura 11**: il prodotto di ogni Visualizza GridView s `ProductID`, `ProductName`, e `UnitPrice` valori ([fare clic per visualizzare l'immagine ingrandita](querying-data-with-the-sqldatasource-control-cs/_static/image17.gif))


Quando viene visitata GridView richiama il controllo origine dati s `Select()` metodo. Quando si stava usando il controllo ObjectDataSource, questa chiamata di `ProductsBLL` classe s `GetProducts()` metodo. Con SqlDataSource, tuttavia, il `Select()` metodo stabilisce una connessione al database specificato e i problemi di `SelectCommand` (`SELECT [ProductID], [ProductName], [UnitPrice] FROM [Products]`, in questo esempio). SqlDataSource restituisce i risultati che quindi GridView enumera, creazione di una riga in GridView per ogni record di database restituiti.

## <a name="the-built-in-data-web-control-features-and-the-sqldatasource-control"></a>Le funzionalità di controllo Web di dati incorporati e il controllo SqlDataSource

In generale, le funzionalità inerenti ai dati di che paging, ordinamento, la modifica di controlli Web l'eliminazione, inserimento e così via sono specifiche del controllo Web di dati e non dipendono dal controllo origine dati utilizzato. Vale a dire GridView utilizzabili relativo predefinito di paging, ordinamento, modifica ed eliminazione se è associato a un ObjectDataSource o un SqlDataSource. Tuttavia, alcune funzionalità di controllo Web di dati sono riservati per il controllo origine dati in uso o per la configurazione del controllo s origine dati.

Ad esempio, nel [in modo efficiente il Paging tramite quantità di dati di grandi dimensioni](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs.md) esercitazione è stato illustrato come, per impostazione predefinita, la logica di paging per i dati Web controlli gestire restituisce *tutti* record sottostante origine dati e quindi visualizzano solo il subset appropriato di record specificato l'indice della pagina corrente e il numero di record da visualizzare per pagina. Questo modello è molto inefficiente quando il paging del set di risultati sufficientemente grande. Fortunatamente, ObjectDataSource può essere configurati per supportare il paging personalizzato, che restituisce solo il subset preciso di record da visualizzare. Controllo SqlDataSource, tuttavia, non è altrettanto le proprietà per implementare il paging personalizzato.

Un altro abbastanza particolare con ordinamento e paging si verifica con SqlDataSource. Per impostazione predefinita, i dati restituiti da un SqlDataSource del pool di paging o ordinati tramite il controllo GridView. Per dimostrare questo concetto, selezionare le opzioni di abilitare il Paging e Abilita ordinamento nel controllo GridView s smart tag `Querying.aspx` e verificare che funzioni come previsto.

Ordinamento e paging funziona perché SqlDataSource recupera i dati del database in un DataSet fortemente tipizzato. Che è possibile ottenere il numero totale di record restituiti dalla query un aspetto essenziale per implementare il paging del set di dati. Inoltre, è possibile ordinare i risultati di s set di dati tramite un oggetto DataView. Queste funzionalità vengono usate automaticamente da SqlDataSource quando le richieste di GridView del pool di paging o dati ordinati.

SqlDataSource può essere configurati per restituire un oggetto DataReader anziché un set di dati modificando il relativo [ `DataSourceMode` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.datasourcemode.aspx) da `DataSet` (predefinito) per `DataReader`. Tramite un oggetto DataReader potrebbe preferito nelle situazioni quando si passa i risultati di s SqlDataSource al codice esistente che prevede un DataReader. Inoltre, poiché DataReader sono oggetti notevolmente più semplici rispetto a set di dati, offrono prestazioni migliori. Se si apporta questa modifica, tuttavia, non è possibile ordinare il controllo Web dati né pagina poiché SqlDataSource non è possibile verificare il numero di record viene restituito dalla query e neppure DataReader offrono le tecniche per l'ordinamento dei dati restituiti.

## <a name="step-4-using-a-custom-sql-statement-or-stored-procedure"></a>Passaggio 4: Utilizzando un'istruzione SQL personalizzata o una Stored Procedure

Quando si configura il controllo SqlDataSource, la query utilizzata per restituire dati può essere specificata in uno dei due approcci come una stored procedure o un'istruzione SQL personalizzata o come le colonne di una tabella o vista esistente. Nel passaggio 2 viene esaminato selezionando le colonne dal `Products` tabella. Consente di valutare l'utilizzo di un'istruzione SQL personalizzata s.

Aggiungere un altro controllo GridView il `Querying.aspx` pagina e scegliere di creare una nuova origine dati dall'elenco a discesa nello smart tag. Successivamente, indicano che i dati verranno prelevati da un database si creerà un nuovo controllo SqlDataSource. Denominare il controllo `ProductsWithCategoryInfoDataSource`.


![Creare un nuovo controllo SqlDataSource denominato ProductsWithCategoryInfoDataSource](querying-data-with-the-sqldatasource-control-cs/_static/image18.gif)

**Figura 12**: creare un nuovo controllo SqlDataSource denominato`ProductsWithCategoryInfoDataSource`


Nella schermata successiva viene chiesto di specificare il database. Come è stato fatto tornare nella figura 7, selezionare il `NORTHWINDConnectionString` dall'elenco a discesa elenco e fare clic su Avanti. Schermata di istruzione Select della configurazione, scegliere di specificare un pulsante di opzione di stored procedure o un'istruzione SQL personalizzata e fare clic su Avanti. Verrà visualizzata la schermata di Stored procedure o definire istruzioni personalizzate, che offre schede: SELECT, UPDATE, INSERT e DELETE. In ogni scheda è possibile immettere un'istruzione SQL personalizzata nella casella di testo o scegliere una stored procedure dall'elenco a discesa. In questa esercitazione verranno esaminati immettendo un'istruzione SQL personalizzata. l'esercitazione successiva include un esempio che utilizza una stored procedure.


![Immettere un'istruzione SQL personalizzata o selezionare una Stored Procedure](querying-data-with-the-sqldatasource-control-cs/_static/image19.gif)

**Figura 13**: consente di immettere un'istruzione SQL personalizzata o selezionare una Stored Procedure


L'istruzione SQL personalizzata può essere immesso manualmente nella casella di testo oppure può essere costruito graficamente facendo clic sul pulsante Generatore di Query. Dal generatore di Query o la casella di testo, utilizzare la seguente query per restituire il `ProductID` e `ProductName` i campi dal `Products` tabella utilizzando un `JOIN` per recuperare il prodotto s `CategoryName` dal `Categories` tabella:


[!code-sql[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample4.sql)]


![È possibile graficamente costruire la Query utilizzando il generatore di Query](querying-data-with-the-sqldatasource-control-cs/_static/image20.gif)

**Nella figura 14**: È possibile costruire graficamente la Query utilizzando il generatore di Query


Dopo aver specificato la query, fare clic su Avanti per passare alla schermata Test Query. Fare clic su Fine per completare la procedura guidata SqlDataSource.

Dopo aver completato la procedura guidata, GridView disporrà di tre BoundField aggiungervi la visualizzazione di `ProductID`, `ProductName`, e `CategoryName` colonne restituite dalla query e risultante nel markup dichiarativo seguente:


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample5.aspx)]


[![GridView Mostra ogni ID prodotto s, il nome di categoria, nome e l'oggetto associato](querying-data-with-the-sqldatasource-control-cs/_static/image22.gif)](querying-data-with-the-sqldatasource-control-cs/_static/image21.gif)

**Figura 15**: ID s il GridView Mostra ogni prodotto, nome e il nome di categoria associata ([fare clic per visualizzare l'immagine ingrandita](querying-data-with-the-sqldatasource-control-cs/_static/image23.gif))


## <a name="summary"></a>Riepilogo

In questa esercitazione è stato illustrato come eseguire una query e visualizzare i dati utilizzando il controllo SqlDataSource. Ad esempio ObjectDataSource, SqlDataSource funge da proxy, fornendo un approccio dichiarativo per l'accesso ai dati. La proprietà specifica il database a cui connettersi e SQL `SELECT` per l'esecuzione di query; possono essere specificate tramite la finestra proprietà o tramite la procedura guidata Configura origine dati.

Il `SELECT` esempi di query sono state esaminate in questa esercitazione tutti i record restituiti dalla query specificata. Il controllo SqlDataSource, tuttavia, può includere un `WHERE` clausola con parametri i cui valori vengono assegnati a livello di codice o automaticamente vengono estratti da un'origine specificata. Come creare e utilizzare query con parametri nella prossima esercitazione verrà esaminato!

Buona programmazione!

## <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [L'accesso ai dati di Database relazionale](http://aspnet.4guysfromrolla.com/articles/022206-1.aspx)
- [Cenni preliminari sul controllo SqlDataSource](https://msdn.microsoft.com/library/dz12d98w.aspx)
- [Esercitazioni delle Guide rapide di ASP.NET: Il controllo SqlDataSource](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/data/sqldatasource.aspx)
- [Il file Web. config `<connectionStrings>` elemento](https://msdn.microsoft.com/library/bf7sd233.aspx)
- [Riferimento di stringa di connessione database](http://www.connectionstrings.com/)

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Lead revisori per questa esercitazione sono stati Connery Susan Bernadette Leigh e David Suru. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[avanti](using-parameterized-queries-with-the-sqldatasource-cs.md)
