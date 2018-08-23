---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/working-with-computed-columns-cs
title: Utilizzo di colonne calcolate (c#) | Microsoft Docs
author: rick-anderson
description: Quando si crea una tabella di database, Microsoft SQL Server consente di definire una colonna calcolata il cui valore viene calcolato da un'espressione che, in genere referen...
ms.author: riande
ms.date: 08/03/2007
ms.assetid: 57459065-ed7c-4dfe-ac9c-54c093abc261
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/working-with-computed-columns-cs
msc.type: authoredcontent
ms.openlocfilehash: 5ef548c627cd40159bb3961f479401657a2ac394
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827514"
---
<a name="working-with-computed-columns-c"></a>Utilizzo di colonne calcolate (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare il codice](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_71_CS.zip) o [Scarica il PDF](working-with-computed-columns-cs/_static/datatutorial71cs1.pdf)

> Quando si crea una tabella di database, Microsoft SQL Server consente di definire una colonna calcolata il cui valore viene calcolato da un'espressione che fa riferimento in genere altri valori nel record del database stesso. Tali valori sono di sola lettura a livello di database, che richiede considerazioni speciali quando si lavora con gli oggetti TableAdapter. In questa esercitazione sono come le sfide poste dalle colonne calcolate.


## <a name="introduction"></a>Introduzione

Microsoft SQL Server consente  *[le colonne calcolate](https://msdn.microsoft.com/library/ms191250.aspx)*, quali sono le colonne i cui valori vengono calcolati da un'espressione che in genere fa riferimento a valori di altre colonne nella stessa tabella. Ad esempio, un modello di dati di rilevamento di tempo potrebbe creare una tabella denominata `ServiceLog` con colonne incluse `ServicePerformed`, `EmployeeID`, `Rate`, e `Duration`, tra gli altri. Anche se l'importo dovuto per ogni servizio (in corso la frequenza moltiplicata per la durata) può essere calcolato tramite una pagina web o altre interfacce a livello di codice, potrebbe essere utile per includere una colonna nel `ServiceLog` tabella denominata `AmountDue` che ha segnalato questo informazioni. Questa colonna può essere creata come una normale colonna, ma dovranno essere aggiornati in qualsiasi momento il `Rate` o `Duration` valori della colonna modificati. Un approccio migliore sarebbe per rendere il `AmountDue` una colonna calcolata utilizzando l'espressione di colonna `Rate * Duration`. Questa operazione causerebbe SQL Server calcolare automaticamente il `AmountDue` valore della colonna ogni volta che vi viene fatto riferimento in una query.

Poiché un valore di colonna calcolata s è determinato da un'espressione, tali colonne sono di sola lettura e pertanto non possono essere assegnati valori ad essi nel `INSERT` o `UPDATE` istruzioni. Tuttavia, quando le colonne calcolate sono parte della query principale per un oggetto TableAdapter che Usa istruzioni SQL ad hoc, sono incluse automaticamente nel generata automaticamente `INSERT` e `UPDATE` istruzioni. Di conseguenza, i TableAdapter `INSERT` e `UPDATE` query e `InsertCommand` e `UpdateCommand` proprietà devono essere aggiornate per rimuovere i riferimenti alle colonne calcolate.

Uno dei problemi di utilizzo di colonne con un TableAdapter che Usa istruzioni SQL ad-hoc calcolate è che i TableAdapter `INSERT` e `UPDATE` le query vengono rigenerate automaticamente ogni volta che viene completata la configurazione guidata TableAdapter. Di conseguenza, le colonne calcolate rimosse manualmente dal `INSERT` e `UPDATE` query verrà visualizzato nuovamente se la procedura guidata viene rieseguita. Anche se gli oggetti TableAdapter che utilizzano le stored procedure desidero soggette a questa inconvenienti, hanno i propri comportamenti anomali vengono descritte nel passaggio 3.

In questa esercitazione si aggiungerà una colonna calcolata per il `Suppliers` tabella nel database Northwind e quindi creare un oggetto TableAdapter corrispondente per usare questa tabella e la relativa colonna calcolata. Si avrà il TableAdapter utilizzare stored procedure anziché le istruzioni SQL ad hoc in modo che i nostri t sono personalizzazioni persa quando viene utilizzata la configurazione guidata TableAdapter.

Introduzione a ti permettono di s.

## <a name="step-1-adding-a-computed-column-to-thesupplierstable"></a>Passaggio 1: Aggiunta di una colonna calcolata per il`Suppliers`tabella

Il database Northwind non dispone le colonne calcolate in modo che è necessario aggiungere uno noi stessi. Per questa esercitazione lasciare s aggiungere una colonna calcolata per il `Suppliers` tabella denominata `FullContactName` che restituisce il nome di contatto s, titolo e l'azienda che lavorano per il formato seguente: `ContactName` (`ContactTitle`, `CompanyName`). Questa calcolata colonna potrebbe essere utilizzata nel report quando si visualizzano informazioni sui fornitori.

Iniziare aprendo il `Suppliers` definizione di tabella facendo clic su di `Suppliers` tabella in Esplora Server e scegliendo Apri definizione tabella dal menu di scelta rapida. Le colonne della tabella e le relative proprietà, ad esempio tipo di dati, verrà visualizzata se consentono `NULL` s e così via. Per aggiungere una colonna calcolata, iniziare digitando il nome della colonna nella definizione della tabella. Immettere quindi la relativa espressione nella casella di testo (Formula) nella sezione specifica della colonna calcolata nella finestra proprietà di colonna (vedere la figura 1). Assegnare un nome di colonna calcolata `FullContactName` e utilizzare l'espressione seguente:


[!code-sql[Main](working-with-computed-columns-cs/samples/sample1.sql)]

Si noti che le stringhe possono essere concatenate in SQL usando il `+` operatore. Il `CASE` istruzione può essere utilizzata come un'istruzione condizionale in un linguaggio di programmazione tradizionali. Nell'espressione sopra indicata la `CASE` istruzione può essere letto come: se `ContactTitle` non è `NULL` quindi restituiti il `ContactTitle` valore concatenato con una virgola, in caso contrario generare nulla. Per altre informazioni sull'utilità delle `CASE` istruzione, vedere [potenza di SQL `CASE` istruzioni](http://www.4guysfromrolla.com/webtech/102704-1.shtml).

> [!NOTE]
> Invece di usare una `CASE` istruzione qui, si sarebbe potuto in alternativa usare `ISNULL(ContactTitle, '')`. [`ISNULL(checkExpression, replacementValue)`](https://msdn.microsoft.com/library/ms184325.aspx) Restituisce *checkExpression* se è diverso da NULL, in caso contrario, restituisce *replacementValue*. Mentre uno `ISNULL` oppure `CASE` funzionerà in questo caso, esistono scenari più complessi in cui la flessibilità del `CASE` istruzione non possa essere trovata da `ISNULL`.


Dopo aver aggiunto la colonna calcolata la schermata dovrebbe essere simile all'immagine riportata nella figura 1.


[![Aggiungere una colonna calcolata denominata FullContactName alla tabella Suppliers](working-with-computed-columns-cs/_static/image2.png)](working-with-computed-columns-cs/_static/image1.png)

**Figura 1**: aggiungere una colonna calcolata denominata `FullContactName` per il `Suppliers` tabella ([fare clic per visualizzare l'immagine con dimensioni normali](working-with-computed-columns-cs/_static/image3.png))


Dopo aver creato la colonna calcolata e immettere la relativa espressione, salvare le modifiche alla tabella facendo clic sull'icona Salva nella barra degli strumenti, premendo Ctrl + S oppure selezionando il menu File e scegliendo Salva `Suppliers`.

Salvataggio della tabella deve essere aggiornata Esplora Server, tra cui la colonna appena aggiunta nel `Suppliers` elenco colonne di tabella s. Inoltre, l'espressione immessa nella casella di testo (Formula) regolerà automaticamente a un'espressione equivalente che rimuove spazi vuoti non necessari, racchiuso tra parentesi quadre i nomi di colonna (`[]`) e include le parentesi per mostrare in modo più esplicito l'ordine delle operazioni:


[!code-sql[Main](working-with-computed-columns-cs/samples/sample2.sql)]

Per altre informazioni su colonne calcolate in Microsoft SQL Server, vedere la [documentazione tecnica](https://msdn.microsoft.com/library/ms191250.aspx). Consultare il [procedura: specificare le colonne calcolate](https://msdn.microsoft.com/library/ms188300.aspx) per una procedura dettagliata di creazione di colonne calcolate.

> [!NOTE]
> Per impostazione predefinita, le colonne calcolate non sono archiviate fisicamente nella tabella ma vengono invece ricalcolate ogni volta che vi viene fatto riferimento in una query. Selezionando la casella di controllo viene reso persistente, si può però istruire SQL Server per archiviare fisicamente la colonna calcolata nella tabella. In questo modo consente a un indice da creare la colonna calcolata, che consente di migliorare le prestazioni delle query che utilizzano il valore della colonna calcolata nella loro `WHERE` clausole. Visualizzare [creazione di indici per colonne calcolate](https://msdn.microsoft.com/library/ms189292.aspx) per altre informazioni.


## <a name="step-2-viewing-the-computed-column-s-values"></a>Passaggio 2: Visualizzare i valori di colonna calcolata s

Prima di iniziare lavoro nel livello di accesso ai dati, ti permettono di s richiedere un minuto per visualizzare il `FullContactName` valori. Da Esplora Server, fare clic su di `Suppliers` nome tabella e scegliere Nuova Query dal menu di scelta rapida. Verrà visualizzata una finestra di Query che consente di scegliere quali le tabelle da includere nella query. Aggiungere il `Suppliers` di tabella e fare clic su Chiudi. Successivamente, controllare la `CompanyName`, `ContactName`, `ContactTitle`, e `FullContactName` colonne dalla tabella Suppliers. Infine, selezionare l'icona punto esclamativo rosso nella barra degli strumenti per eseguire la query e visualizzare i risultati.

Come illustrato nella figura 2, i risultati includono `FullContactName`, che elenca i `CompanyName`, `ContactName`, e `ContactTitle` colonne usando il formato ldquo;`ContactName` (`ContactTitle`, `CompanyName`) .


[![Il FullContactName Usa il formato ContactName (ContactTitle, CompanyName)](working-with-computed-columns-cs/_static/image5.png)](working-with-computed-columns-cs/_static/image4.png)

**Figura 2**: il `FullContactName` Usa il formato `ContactName` (`ContactTitle`, `CompanyName`) ([fare clic per visualizzare l'immagine con dimensioni normali](working-with-computed-columns-cs/_static/image6.png))


## <a name="step-3-adding-thesupplierstableadapterto-the-data-access-layer"></a>Passaggio 3: Aggiunta di`SuppliersTableAdapter`a livello di accesso ai dati

Per usare le informazioni del fornitore dell'applicazione è necessario innanzitutto creare un oggetto TableAdapter e DataTable in nostro DAL. In teoria, ciò verrebbe eseguito usando la stessa procedura semplice esaminata nelle esercitazioni precedenti. Lavorare con le colonne calcolate, tuttavia, introduce alcuni rughe che meritano discussione.

Se si usa un oggetto TableAdapter che Usa istruzioni SQL ad hoc, è sufficiente includere la colonna calcolata nella query TableAdapter s principale tramite la configurazione guidata TableAdapter. Questa operazione, tuttavia, verrà generato automaticamente `INSERT` e `UPDATE` istruzioni che includono la colonna calcolata. Se si prova a eseguire uno di questi metodi, una `SqlException` con il messaggio della colonna *ColumnName* non può essere modificato perché è una colonna calcolata o verrà generato il risultato di un operatore UNION. Mentre il `INSERT` e `UPDATE` istruzione può essere modificata manualmente tramite i TableAdapter `InsertCommand` e `UpdateCommand` proprietà, queste personalizzazioni andranno perse ogni volta che la configurazione guidata TableAdapter viene rieseguita.

A causa di inconvenienti degli oggetti TableAdapter che usano istruzioni SQL ad hoc, è consigliabile che usiamo le stored procedure quando si lavora con le colonne calcolate. Se si Usa stored procedure esistenti, è sufficiente configurare il TableAdapter come descritto nel [utilizzando Stored procedure esistenti per DataSet tipizzata s TableAdapter](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) esercitazione. Se si ha la configurazione guidata TableAdapter creare le stored procedure per l'utente, tuttavia, è importante inizialmente omettere le colonne calcolate dalla query principale. Se si include una colonna calcolata nella query principali, la configurazione guidata TableAdapter indicherà, dopo il completamento, che non è possibile creare le stored procedure corrispondente. In breve, è necessario configurare inizialmente il TableAdapter utilizzando una query principale privi di colonna calcolata e quindi aggiornare manualmente la stored procedure corrispondente e ai TableAdapter `SelectCommand` per includere la colonna calcolata. Questo approccio è simile a quella usata nel [aggiornamento del TableAdapter per l'uso](updating-the-tableadapter-to-use-joins-cs.md)`JOIN`*s* esercitazione.

Per questa esercitazione, lasciare s aggiungere un nuovo TableAdapter, che verrà automaticamente create le stored procedure per noi. Di conseguenza, è necessario omettere inizialmente il `FullContactName` colonna calcolata dalla query principale.

Iniziare aprendo il `NorthwindWithSprocs` set di dati nel `~/App_Code/DAL` cartella. Fare doppio clic nella finestra di progettazione e, da menu di scelta rapida, scegliere di aggiungere un nuovo TableAdapter. Verrà avviata la configurazione guidata TableAdapter. Specificare il database per eseguire query sui dati da (`NORTHWNDConnectionString` da `Web.config`) e fare clic su Avanti. Poiché le stored procedure per l'esecuzione di query o la modifica non è stata ancora creata la `Suppliers` di tabella, selezionare la creazione di nuove stored procedure opzione in modo che la procedura guidata verrà creati automaticamente e fare clic su Avanti.


[![Scegliere Crea nuove stored procedure. opzione](working-with-computed-columns-cs/_static/image8.png)](working-with-computed-columns-cs/_static/image7.png)

**Figura 3**: scegliere Crea nuove stored procedure. opzione ([fare clic per visualizzare l'immagine con dimensioni normali](working-with-computed-columns-cs/_static/image9.png))


Il passaggio successivo richiede per la query principale. Immettere la query seguente, che restituisce il `SupplierID`, `CompanyName`, `ContactName`, e `ContactTitle` colonne per ogni fornitore. Si noti che questa query esclude intenzionalmente la colonna calcolata (`FullContactName`); verrà aggiornata la stored procedure corrispondente per includere tale colonna nel passaggio 4.


[!code-sql[Main](working-with-computed-columns-cs/samples/sample3.sql)]

Dopo aver immesso la query principale e fare clic su Avanti, la procedura guidata consente di assegnare un nome di quattro stored procedure che verrà generato. Assegnare un nome queste stored procedure `Suppliers_Select`, `Suppliers_Insert`, `Suppliers_Update`, e `Suppliers_Delete`, come illustrato nella figura 4.


[![Personalizzare i nomi delle Stored procedure generata automaticamente](working-with-computed-columns-cs/_static/image11.png)](working-with-computed-columns-cs/_static/image10.png)

**Figura 4**: personalizzare i nomi delle Stored procedure Auto-Generated ([fare clic per visualizzare l'immagine con dimensioni normali](working-with-computed-columns-cs/_static/image12.png))


Il passaggio successivo della procedura guidata consente di denominare i metodi di s TableAdapter e specificare gli schemi utilizzati per accedere e aggiornare i dati. Lasciare tutte le tre caselle di controllo selezionate, ma non rinominare i `GetData` metodo `GetSuppliers`. Fare clic su Fine per completare la procedura guidata.


[![Rinominare il metodo GetData per GetSuppliers](working-with-computed-columns-cs/_static/image14.png)](working-with-computed-columns-cs/_static/image13.png)

**Figura 5**: rinominare i `GetData` metodo `GetSuppliers` ([fare clic per visualizzare l'immagine con dimensioni normali](working-with-computed-columns-cs/_static/image15.png))


Facendo clic su Fine, la procedura guidata verrà creare quattro stored procedure e aggiungere il TableAdapter e DataTable corrispondente al set di dati tipizzato.

## <a name="step-4-including-the-computed-column-in-the-tableadapter-s-main-query"></a>Passaggio 4: Includere la colonna calcolata nella Query principali s TableAdapter

È ora necessario Aggiorna oggetto TableAdapter e DataTable creato nel passaggio 3 per includere il `FullContactName` colonna calcolata. Ciò comporta due passaggi:

1. L'aggiornamento di `Suppliers_Select` stored procedure per restituire il `FullContactName` colonna calcolata, e
2. L'aggiornamento di DataTable per includere un corrispondente `FullContactName` colonna.

Per iniziare, passare a Esplora Server e il drill-down nella cartella di Stored procedure. Aprire il `Suppliers_Select` stored procedure e aggiornamento il `SELECT` query per includere il `FullContactName` colonna calcolata:


[!code-sql[Main](working-with-computed-columns-cs/samples/sample4.sql)]

Salvare le modifiche apportate alla stored procedure facendo clic sull'icona Salva nella barra degli strumenti, premendo Ctrl + S o scegliendo Salva `Suppliers_Select` opzione dal menu File.

Successivamente, tornare alla finestra di progettazione set di dati, fare clic su di `SuppliersTableAdapter`e scegliere Configura dal menu di scelta rapida. Si noti che il `Suppliers_Select` colonna include ora il `FullContactName` nella propria raccolta di colonne di dati della colonna.


[![Eseguire la configurazione guidata s TableAdapter per aggiornare le colonne s DataTable](working-with-computed-columns-cs/_static/image17.png)](working-with-computed-columns-cs/_static/image16.png)

**Figura 6**: eseguire Configurazione guidata per aggiornare gli oggetti DataTable colonne s TableAdapter ([fare clic per visualizzare l'immagine con dimensioni normali](working-with-computed-columns-cs/_static/image18.png))


Fare clic su Fine per completare la procedura guidata. Questo aggiungerà automaticamente una colonna corrispondente per il `SuppliersDataTable`. La configurazione guidata TableAdapter è abbastanza intelligente da rilevare che il `FullContactName` colonna è una colonna calcolata e pertanto di sola lettura. Di conseguenza, imposta la colonna 1!s `ReadOnly` proprietà `true`. A questo scopo, selezionare la colonna dal `SuppliersDataTable` e quindi passare alla finestra delle proprietà (vedere la figura 7). Si noti che il `FullContactName` s' `DataType` e `MaxLength` proprietà vengono inoltre impostate di conseguenza.


[![La colonna FullContactName è contrassegnata come sola lettura](working-with-computed-columns-cs/_static/image20.png)](working-with-computed-columns-cs/_static/image19.png)

**Figura 7**: il `FullContactName` colonna è contrassegnata come di sola lettura ([fare clic per visualizzare l'immagine con dimensioni normali](working-with-computed-columns-cs/_static/image21.png))


## <a name="step-5-adding-agetsupplierbysupplieridmethod-to-the-tableadapter"></a>Passaggio 5: Aggiunta di un`GetSupplierBySupplierID`all'oggetto TableAdapter (metodo)

Per questa esercitazione si creerà una pagina ASP.NET che consente di visualizzare i fornitori in una griglia aggiornabile. Nelle precedenti esercitazioni sono state aggiornate un singolo record dal livello della logica di Business recuperando nuovamente i record specifico DAL come DataTable fortemente tipizzato, l'aggiornamento delle relative proprietà e quindi l'invio di DataTable aggiornate al livello per propagare le modifiche apportate a il database. Per eseguire il primo passaggio - recuperare il record viene aggiornato DAL: è necessario innanzitutto aggiungere un `GetSupplierBySupplierID(supplierID)` DAL metodo.

Fare clic su di `SuppliersTableAdapter` nella struttura del set di dati e scegliere l'opzione di Query aggiunta dal menu di scelta rapida. Come è stato fatto nel passaggio 3, consentire alla procedura guidata genera una nuova stored procedure per noi selezionando l'opzione di creazione nuova stored procedure (vedere la figura 3 per visualizzare uno screenshot di questo passaggio della procedura guidata). Poiché questo metodo restituisce un record con più colonne, indicano che si desidera utilizzare una query SQL che è un'istruzione SELECT che restituisce le righe e fare clic su Avanti.


[![Scegliere il SELECT che restituisce righe opzione](working-with-computed-columns-cs/_static/image23.png)](working-with-computed-columns-cs/_static/image22.png)

**Figura 8**: scegliere la SELECT che restituisce righe opzione ([fare clic per visualizzare l'immagine con dimensioni normali](working-with-computed-columns-cs/_static/image24.png))


Il passaggio successivo richiede per la query da usare per questo metodo. Immettere le informazioni seguenti, che restituisce gli stessi campi di dati come query principale, ma per un particolare fornitore.


[!code-sql[Main](working-with-computed-columns-cs/samples/sample5.sql)]

Nella schermata successiva viene chiesto di specificare un nome di stored procedure che sarà generato automaticamente. Nome di questa stored procedure `Suppliers_SelectBySupplierID` e fare clic su Avanti.


[![Nome Suppliers_SelectBySupplierID la Stored Procedure](working-with-computed-columns-cs/_static/image26.png)](working-with-computed-columns-cs/_static/image25.png)

**Figura 9**: nome della Stored Procedure `Suppliers_SelectBySupplierID` ([fare clic per visualizzare l'immagine con dimensioni normali](working-with-computed-columns-cs/_static/image27.png))


Infine, le istruzioni della procedura guidata us per i dati di accedere ai modelli e i nomi dei metodi da usare per l'oggetto TableAdapter. Lasciare entrambe le caselle di controllo selezionate, ma non rinominare i `FillBy` e `GetDataBy` metodi `FillBySupplierID` e `GetSupplierBySupplierID`, rispettivamente.


[![Nome FillBySupplierID i metodi TableAdapter e GetSupplierBySupplierID](working-with-computed-columns-cs/_static/image29.png)](working-with-computed-columns-cs/_static/image28.png)

**Figura 10**: nome i metodi TableAdapter `FillBySupplierID` e `GetSupplierBySupplierID` ([fare clic per visualizzare l'immagine con dimensioni normali](working-with-computed-columns-cs/_static/image30.png))


Fare clic su Fine per completare la procedura guidata.

## <a name="step-6-creating-the-business-logic-layer"></a>Passaggio 6: Creazione di livello della logica di Business

Prima di creare una pagina ASP.NET che usa la colonna calcolata creata nel passaggio 1, è innanzitutto necessario aggiungere i metodi corrispondenti nel livello BLL. La pagina ASP.NET, che verrà creato nel passaggio 7, consentirà agli utenti di visualizzare e modificare i suoi fornitori. Pertanto, è necessario il livello BLL fornire, come minimo, un metodo per ottenere tutti i fornitori e un altro per aggiornare un particolare fornitore.

Creare un nuovo file di classe denominato `SuppliersBLLWithSprocs` nella `~/App_Code/BLL` cartella e aggiungere il codice seguente:


[!code-csharp[Main](working-with-computed-columns-cs/samples/sample6.cs)]

Come le altre classi BLL `SuppliersBLLWithSprocs` ha un `protected` `Adapter` proprietà che restituisce un'istanza del `SuppliersTableAdapter` classe insieme a due `public` metodi: `GetSuppliers` e `UpdateSupplier`. Il `GetSuppliers` metodo viene chiamato e restituisce il `SuppliersDataTable` restituito dalla corrispondente `GetSupplier` metodo nel livello di accesso ai dati. Il `UpdateSupplier` recupera informazioni relative in corso l'aggiornamento tramite una chiamata a s DAL fornitore specifico `GetSupplierBySupplierID(supplierID)` (metodo). Quindi aggiorna il `CategoryName`, `ContactName`, e `ContactTitle` proprietà ed esegue il commit di queste modifiche al database chiamando il livello di accesso ai dati di s `Update` , passando nella versione modificata `SuppliersRow` oggetto.

> [!NOTE]
> Ad eccezione di `SupplierID` e `CompanyName`, tutte le colonne nella tabella Suppliers consentono `NULL` valori. Pertanto, se passato `contactName` o `contactTitle` i parametri sono `null` , dobbiamo impostare corrispondente `ContactName` e `ContactTitle` le proprietà per un `NULL` database valore usando il `SetContactNameNull` e`SetContactTitleNull`metodi di, rispettivamente.


## <a name="step-7-working-with-the-computed-column-from-the-presentation-layer"></a>Passaggio 7: Uso con la colonna calcolata dal livello di presentazione

Con la colonna calcolata aggiunta al `Suppliers` tabella e DAL e BLL aggiornato di conseguenza, si è pronti a compilare una pagina ASP.NET che funziona con il `FullContactName` colonna calcolata. Iniziare aprendo il `ComputedColumns.aspx` nella pagina di `AdvancedDAL` cartelle e trascinare un controllo GridView dalla casella degli strumenti nella finestra di progettazione. Impostare la s GridView `ID` proprietà `Suppliers` e, dal suo smart tag, associarlo a un nuovo oggetto ObjectDataSource denominato `SuppliersDataSource`. Configurare ObjectDataSource per usare il `SuppliersBLLWithSprocs` classe è stato aggiunto il nuovo nel passaggio 6 e fare clic su Avanti.


[![Configurare ObjectDataSource per usare la classe SuppliersBLLWithSprocs](working-with-computed-columns-cs/_static/image32.png)](working-with-computed-columns-cs/_static/image31.png)

**Figura 11**: configurare ObjectDataSource per usare il `SuppliersBLLWithSprocs` classe ([fare clic per visualizzare l'immagine con dimensioni normali](working-with-computed-columns-cs/_static/image33.png))


Sono disponibili solo due metodi definiti nel `SuppliersBLLWithSprocs` classe: `GetSuppliers` e `UpdateSupplier`. Assicurarsi che questi due metodi sono specificati in SELECT e aggiornare le schede, rispettivamente e fare clic su Fine per completare la configurazione di ObjectDataSource.

Dopo avere completato la configurazione guidata origine dati, Visual Studio aggiungerà un BoundField per ognuno dei campi di dati restituiti. Rimuovere il `SupplierID` BoundField e modificare il `HeaderText` delle proprietà delle `CompanyName`, `ContactName`, `ContactTitle`, e `FullContactName` BoundField per società, nome contatto, titolo e il nome di contatto completo, rispettivamente. Nello smart tag, la casella Abilita modifica per attivare la funzionalità di modifica predefinita s di GridView.

Oltre ad aggiungere i BoundField di GridView, il completamento della creazione guidata origine dati provoca anche Visual Studio consente di impostare la s ObjectDataSource `OldValuesParameterFormatString` proprietà originale\_{0}. Annullare tale impostazione il valore predefinito, {0} .

Dopo aver apportato queste modifiche per il controllo GridView e ObjectDataSource, il relativo markup dichiarativo dovrebbe essere simile al seguente:


[!code-aspx[Main](working-with-computed-columns-cs/samples/sample7.aspx)]

Successivamente, visitare questa pagina tramite un browser. Come illustrato nella figura 12, ogni fornitore è elencato in una griglia contenente le `FullContactName` colonna, il cui valore è semplicemente la concatenazione delle altre tre colonne formattato come `ContactName` (`ContactTitle`, `CompanyName`).


[![Ogni fornitore è elencata nella griglia](working-with-computed-columns-cs/_static/image35.png)](working-with-computed-columns-cs/_static/image34.png)

**Figura 12**: ogni fornitore è elencata nella griglia ([fare clic per visualizzare l'immagine con dimensioni normali](working-with-computed-columns-cs/_static/image36.png))


Facendo clic sul pulsante Modifica per un particolare fornitore determina un postback e dispone di tale riga viene eseguito il rendering nel relativo tipo di modifica dell'interfaccia (vedere la figura 13). Eseguire il rendering le prime tre colonne in base all'impostazione predefinita dell'interfaccia di modifica - controllo di una casella di testo la cui `Text` proprietà è impostata sul valore del campo dati. Il `FullContactName` colonna, tuttavia, rimane come testo. Quando sono stati aggiunti i BoundField di GridView dopo aver completato la configurazione guidata origine dati, il `FullContactName` s BoundField `ReadOnly` è stata impostata su `true` perché corrispondente `FullContactName` colonna il `SuppliersDataTable` ha relativi `ReadOnly` impostata su `true`. Come indicato nel passaggio 4, il `FullContactName` s `ReadOnly` è stata impostata su `true` perché TableAdapter rilevato che la colonna è una colonna calcolata.


[![La colonna FullContactName non è modificabile](working-with-computed-columns-cs/_static/image38.png)](working-with-computed-columns-cs/_static/image37.png)

**Figura 13**: il `FullContactName` colonna non è modificabile ([fare clic per visualizzare l'immagine con dimensioni normali](working-with-computed-columns-cs/_static/image39.png))


Proseguire e aggiornare il valore di uno o più colonne modificabili e fare clic su Aggiorna. Si noti come il `FullContactName` s valore viene automaticamente aggiornato per riflettere la modifica.

> [!NOTE]
> Il controllo GridView Usa attualmente BoundField per i campi modificabili, determinando il valore predefinito dell'interfaccia di modifica. Poiché il `CompanyName` campo è obbligatorio, deve essere convertito in un TemplateField che include un RequiredFieldValidator. Lasciare come esercizio per il lettore di interesse. Consultare il [aggiunta di controlli di convalida per la modifica e inserimento di interfacce](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) esercitazione per istruzioni dettagliate sulla conversione di un BoundField in un TemplateField e aggiunta di controlli di convalida.


## <a name="summary"></a>Riepilogo

Quando si definisce lo schema per una tabella, Microsoft SQL Server consente l'inclusione di colonne calcolate. Queste sono colonne i cui valori vengono calcolati da un'espressione che in genere fa riferimento a valori di altre colonne nello stesso record. Poiché i valori per le colonne calcolate sono basate su un'espressione, sono di sola lettura e non può essere assegnati un valore in un `INSERT` o `UPDATE` istruzione. Ciò introduce sfide quando si usa una colonna calcolata nella query principali di un oggetto TableAdapter che tenta di generare automaticamente corrispondente `INSERT`, `UPDATE`, e `DELETE` istruzioni.

In questa esercitazione viene illustrato le tecniche per eludere le sfide poste dalle colonne calcolate. In particolare, abbiamo utilizzato le stored procedure nel nostro TableAdapter per superare inconvenienti inerenti alla classe TableAdapter che usano istruzioni SQL ad hoc. Con la configurazione guidata TableAdapter di creare nuove stored procedure, è importante che abbiamo la query principale inizialmente omettere le colonne calcolate perché la loro presenza impedisce la generazione dei dati stored procedure di modifica. Dopo aver configurato inizialmente TableAdapter, relativo `SelectCommand` stored procedure può essere retooled per includere le colonne calcolate.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. I revisori per questa esercitazione sono state Hilton Geisenow e Teresa Murphy. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](adding-additional-datatable-columns-cs.md)
> [Successivo](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs.md)
