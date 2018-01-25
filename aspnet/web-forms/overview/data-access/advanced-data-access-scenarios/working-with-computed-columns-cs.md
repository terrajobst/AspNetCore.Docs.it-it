---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/working-with-computed-columns-cs
title: Utilizzo di colonne calcolate (c#) | Documenti Microsoft
author: rick-anderson
description: Quando si crea una tabella di database, Microsoft SQL Server consente di definire una colonna calcolata il cui valore viene calcolato da un'espressione che in genere referen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: 57459065-ed7c-4dfe-ac9c-54c093abc261
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/working-with-computed-columns-cs
msc.type: authoredcontent
ms.openlocfilehash: 41206f76f9d9ca68971a53d79e84d82349e92333
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="working-with-computed-columns-c"></a>Utilizzo di colonne calcolate (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare codice](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_71_CS.zip) o [Scarica il PDF](working-with-computed-columns-cs/_static/datatutorial71cs1.pdf)

> Quando si crea una tabella di database, Microsoft SQL Server consente di definire una colonna calcolata il cui valore viene calcolato da un'espressione che fa riferimento in genere di altri valori nel record del database stesso. Tali valori sono di sola lettura nel database di, che richiede alcune considerazioni speciali quando si lavora con gli oggetti TableAdapter. In questa esercitazione è imparare a soddisfare le esigenze di derivanti dalle colonne calcolate.


## <a name="introduction"></a>Introduzione

Microsoft SQL Server consente di  *[le colonne calcolate](https://msdn.microsoft.com/library/ms191250.aspx)*, che sono colonne i cui valori sono calcolati da un'espressione che in genere fa riferimento ai valori di altre colonne nella stessa tabella. Ad esempio, un modello di dati di rilevamento di tempo potrebbe creare una tabella denominata `ServiceLog` con colonne incluse `ServicePerformed`, `EmployeeID`, `Rate`, e `Duration`, tra gli altri. Mentre l'importo dovuto per ogni servizio elemento (la velocità di moltiplicato per la durata viene) può essere calcolato tramite una pagina web o un'altra interfaccia a livello di codice, potrebbe essere utile per includere una colonna di `ServiceLog` tabella denominata `AmountDue` che ha segnalato questo informazioni. Questa colonna può essere creata come una normale colonna, ma è necessario aggiornare in qualsiasi momento il `Rate` o `Duration` valori della colonna modificati. Un approccio migliore, è possibile rendere il `AmountDue` una colonna calcolata con l'espressione di colonna `Rate * Duration`. Questa operazione provocherebbe SQL Server calcolare automaticamente il `AmountDue` valore della colonna ogni volta che vi viene fatto riferimento in una query.

Poiché un valore della colonna calcolata s è determinato da un'espressione, tali colonne sono di sola lettura e pertanto non possono essere assegnati valori a essi in `INSERT` o `UPDATE` istruzioni. Tuttavia, quando le colonne calcolate sono parte della query principale per un oggetto TableAdapter che utilizza istruzioni SQL ad hoc, vengono incluse automaticamente nel generata automaticamente `INSERT` e `UPDATE` istruzioni. Di conseguenza, i TableAdapter `INSERT` e `UPDATE` query e `InsertCommand` e `UpdateCommand` proprietà devono essere aggiornate per rimuovere i riferimenti a tutte le colonne calcolate.

Una sfida di utilizzo di colonne con un TableAdapter che utilizza istruzioni SQL ad hoc calcolate è che i TableAdapter `INSERT` e `UPDATE` le query vengono rigenerate automaticamente ogni volta che viene completata la configurazione guidata TableAdapter. Di conseguenza, le colonne calcolate rimosso manualmente dal `INSERT` e `UPDATE` query verrà visualizzato nuovamente se la procedura guidata viene rieseguita. Anche se gli oggetti TableAdapter che utilizzano stored procedure non è stata ridimensionata subire questa vulnerabilità, hanno i propri quirks che si risolverà nel passaggio 3.

In questa esercitazione si aggiungerà una colonna calcolata per il `Suppliers` tabella nel database Northwind e quindi creare un oggetto TableAdapter corrispondente per funzionare con questa tabella e la colonna calcolata. Si avranno il TableAdapter utilizzare stored procedure anziché istruzioni SQL ad hoc in modo che il nostro t personalizzazioni persi quando viene utilizzata la configurazione guidata TableAdapter.

Let s iniziare!

## <a name="step-1-adding-a-computed-column-to-thesupplierstable"></a>Passaggio 1: Aggiunta di una colonna calcolata per il`Suppliers`tabella

Il database Northwind non dispone delle colonne calcolate, pertanto sarà necessario aggiungere una effettuata. Per questa esercitazione consente s aggiungere una colonna calcolata per il `Suppliers` tabella denominata `FullContactName` che restituisce il nome del contatto s, titolo e la società che lavorano per il seguente formato: `ContactName` (`ContactTitle`, `CompanyName`). Questa calcolata colonna può essere utilizzata nei report nella visualizzazione di informazioni sui fornitori.

Aprire il `Suppliers` definizione tabella facendo clic su di `Suppliers` tabella in Esplora Server e scegliendo Apri definizione tabella dal menu di scelta rapida. Le colonne della tabella e le relative proprietà, ad esempio i tipi di dati, verrà visualizzata se consentono `NULL` s e così via. Per aggiungere una colonna calcolata, avviare digitando il nome della colonna nella definizione della tabella. Quindi, immettere l'espressione nella casella di testo (Formula) nella sezione specifica colonna calcolata nella finestra proprietà di colonna (vedere la figura 1). Nome della colonna calcolata `FullContactName` e utilizzare l'espressione seguente:


[!code-sql[Main](working-with-computed-columns-cs/samples/sample1.sql)]

Si noti che le stringhe possono essere concatenate in SQL utilizzando il `+` operatore. Il `CASE` istruzione può essere utilizzata come un'istruzione condizionale in un linguaggio di programmazione tradizionali. Nell'espressione di `CASE` istruzione può essere letto come: se `ContactTitle` non `NULL` quindi output il `ContactTitle` valore concatenato con una virgola, in caso contrario emit nulla. Per ulteriori informazioni sulle utilità del `CASE` istruzione, vedere [Power di SQL `CASE` istruzioni](http://www.4guysfromrolla.com/webtech/102704-1.shtml).

> [!NOTE]
> Anziché utilizzare un `CASE` istruzione qui avremmo potuto in alternativa utilizzare `ISNULL(ContactTitle, '')`. [`ISNULL(checkExpression, replacementValue)`](https://msdn.microsoft.com/library/ms184325.aspx)Restituisce *checkExpression* se è non NULL, in caso contrario restituisce *replacementValue*. Durante uno `ISNULL` o `CASE` funzionerà in questo caso, vi sono più complessi scenari in cui la flessibilità del `CASE` istruzione non può corrispondere a `ISNULL`.


Dopo aver aggiunto la colonna calcolata la schermata dovrebbe essere simile all'immagine riportata nella figura 1.


[![Aggiungere una colonna calcolata denominata FullContactName alla tabella Suppliers](working-with-computed-columns-cs/_static/image2.png)](working-with-computed-columns-cs/_static/image1.png)

**Figura 1**: aggiungere una colonna calcolata denominata `FullContactName` per il `Suppliers` tabella ([fare clic per visualizzare l'immagine ingrandita](working-with-computed-columns-cs/_static/image3.png))


Dopo aver creato la colonna calcolata e immettere la relativa espressione, salvare le modifiche alla tabella facendo clic sull'icona Salva nella barra degli strumenti, premendo Ctrl + S oppure dal menu File e scegliendo Salva `Suppliers`.

Salvare la tabella è necessario aggiornare Esplora Server, inclusa la colonna appena aggiunto nel `Suppliers` elenco di colonne di tabella s. Inoltre, l'espressione immessa nella casella di testo (Formula) regolerà automaticamente a un'espressione equivalente che rimuove gli spazi vuoti non necessari, racchiuso tra parentesi quadre i nomi di colonna (`[]`) e include le parentesi per mostrare in modo più esplicito l'ordine delle operazioni:


[!code-sql[Main](working-with-computed-columns-cs/samples/sample2.sql)]

Per ulteriori informazioni su colonne calcolate in Microsoft SQL Server, consultare il [documentazione tecnica](https://msdn.microsoft.com/library/ms191250.aspx). Vedere anche il [procedura: specificare le colonne calcolate](https://msdn.microsoft.com/library/ms188300.aspx) per una descrizione dettagliata della creazione di colonne calcolate.

> [!NOTE]
> Per impostazione predefinita, le colonne calcolate non sono archiviate fisicamente nella tabella, ma sono invece ricalcolate ogni volta che vi viene fatto riferimento in una query. Selezionando la casella di controllo è persistente, tuttavia, è possibile indicare di SQL Server per archiviare fisicamente la colonna calcolata nella tabella. In questo modo consente di creare la colonna calcolata, che consente di migliorare le prestazioni delle query che utilizzano il valore di colonna calcolata in un indice loro `WHERE` clausole. Vedere [la creazione di indici per colonne calcolate](https://msdn.microsoft.com/library/ms189292.aspx) per ulteriori informazioni.


## <a name="step-2-viewing-the-computed-column-s-values"></a>Passaggio 2: Visualizzare i valori di colonna calcolata s

Prima di iniziare lavoro nel livello di accesso ai dati, s consentono di richiedere un minuto, per visualizzare il `FullContactName` valori. Da Esplora Server, fare clic su di `Suppliers` nome tabella e scegliere Nuova Query dal menu di scelta rapida. Verrà visualizzata una finestra di Query che richiede di scegliere le tabelle da includere nella query. Aggiungere il `Suppliers` tabella e fare clic su Chiudi. Successivamente, controllare il `CompanyName`, `ContactName`, `ContactTitle`, e `FullContactName` colonne dalla tabella Suppliers. Infine, fare clic sull'icona di punto esclamativo rosso nella barra degli strumenti per eseguire la query e visualizzare i risultati.

Come illustrato nella figura 2, i risultati includono `FullContactName`, quali gli elenchi di `CompanyName`, `ContactName`, e `ContactTitle` colonne utilizzando il formato ldquo;`ContactName` (`ContactTitle`, `CompanyName`) .


[![Il FullContactName utilizza il formato ContactName (ContactTitle, CompanyName)](working-with-computed-columns-cs/_static/image5.png)](working-with-computed-columns-cs/_static/image4.png)

**Figura 2**: il `FullContactName` utilizza il formato `ContactName` (`ContactTitle`, `CompanyName`) ([fare clic per visualizzare l'immagine ingrandita](working-with-computed-columns-cs/_static/image6.png))


## <a name="step-3-adding-thesupplierstableadapterto-the-data-access-layer"></a>Passaggio 3: Aggiunta di`SuppliersTableAdapter`a livello di accesso ai dati

Per utilizzare le informazioni del fornitore dell'applicazione è necessario innanzitutto creare un oggetto TableAdapter e DataTable nel nostro DAL. In teoria, questa verrebbe eseguita utilizzando gli stessi passaggi semplici esaminati nelle esercitazioni precedenti. Tuttavia, lavorare con le colonne calcolate introduce alcuni rughe che meritano discussione.

Se si utilizza un oggetto TableAdapter che utilizza istruzioni SQL ad hoc, è sufficiente includere la colonna calcolata nella query TableAdapter s principale tramite la configurazione guidata TableAdapter. Questa operazione, tuttavia, genererà automaticamente `INSERT` e `UPDATE` le istruzioni che includono la colonna calcolata. Se si tenta di eseguire uno di questi metodi, un `SqlException` con il messaggio, la colonna *ColumnName* non può essere modificato perché è una colonna calcolata o il risultato di un operatore UNION, verrà generato. Mentre il `INSERT` e `UPDATE` istruzione può essere modificata manualmente tramite i TableAdapter `InsertCommand` e `UpdateCommand` proprietà, queste personalizzazioni andranno perse ogni volta che la configurazione guidata TableAdapter viene rieseguita.

A causa della vulnerabilità degli oggetti TableAdapter che Usa istruzioni SQL ad hoc, è consigliabile che le stored procedure viene usata quando si utilizzano colonne calcolate. Se si utilizza stored procedure esistenti, semplicemente configurare il TableAdapter come descritto nel [utilizzando Stored procedure esistenti per gli oggetti TableAdapter s DataSet tipizzato](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) esercitazione. Se si dispone, la configurazione guidata TableAdapter crea le stored procedure per l'utente, tuttavia, è importante inizialmente omettere le colonne calcolate dalla query principale. Se si include una colonna calcolata nella query principale, la configurazione guidata TableAdapter verrà informa, dopo il completamento, che non è possibile creare le stored procedure corrispondente. In breve, è necessario configurare inizialmente il TableAdapter utilizzando una query principale senza colonna calcolata e quindi aggiornare manualmente la stored procedure corrispondente e ai TableAdapter `SelectCommand` per includere la colonna calcolata. Questo approccio è simile a quella usata nel [l'aggiornamento dell'oggetto TableAdapter utilizzare](updating-the-tableadapter-to-use-joins-cs.md)`JOIN`*s* esercitazione.

Consente di s per questa esercitazione, aggiungere un nuovo TableAdapter e fare in modo creare automaticamente le stored procedure per noi. Di conseguenza, è necessario omettere inizialmente il `FullContactName` colonna calcolata dalla query principale.

Aprire il `NorthwindWithSprocs` set di dati nel `~/App_Code/DAL` cartella. Pulsante destro del mouse nella finestra di progettazione e scegliere il menu di scelta rapida aggiungere un nuovo TableAdapter. Verrà avviata la configurazione guidata TableAdapter. Specificare il database per i dati di query (`NORTHWNDConnectionString` da `Web.config`) e fare clic su Avanti. Poiché non è stata ancora creata alcuna stored procedure per l'esecuzione di query o la modifica di `Suppliers` tabella, selezionare la creazione di nuove stored procedure opzione in modo che la procedura guidata verrà creati automaticamente e fare clic su Avanti.


[![Scegliere la crea nuove stored procedure opzione](working-with-computed-columns-cs/_static/image8.png)](working-with-computed-columns-cs/_static/image7.png)

**Figura 3**: scegliere la crea nuove stored procedure opzione ([fare clic per visualizzare l'immagine ingrandita](working-with-computed-columns-cs/_static/image9.png))


Il passaggio successivo ci richiesto per la query principale. Immettere la query seguente, che restituisce il `SupplierID`, `CompanyName`, `ContactName`, e `ContactTitle` colonne per ogni fornitore. Si noti che questa query intenzionalmente consente di omettere la colonna calcolata (`FullContactName`); verrà aggiornato per includere la colonna nel passaggio 4 la stored procedure corrispondente.


[!code-sql[Main](working-with-computed-columns-cs/samples/sample3.sql)]

Dopo aver immesso la query principale e facendo clic su Avanti, la procedura guidata consente di assegnare un nome di quattro stored procedure che verrà generato. Nome di queste stored procedure `Suppliers_Select`, `Suppliers_Insert`, `Suppliers_Update`, e `Suppliers_Delete`, come illustrato nella figura 4.


[![Personalizzare i nomi delle Stored procedure generata automaticamente](working-with-computed-columns-cs/_static/image11.png)](working-with-computed-columns-cs/_static/image10.png)

**Figura 4**: personalizzare i nomi delle Stored procedure Auto-Generated ([fare clic per visualizzare l'immagine ingrandita](working-with-computed-columns-cs/_static/image12.png))


Il passaggio successivo della procedura guidata consente di denominare metodi s dell'oggetto TableAdapter e specificare i modelli utilizzati per accedere e aggiornare dati. Lasciare tutti i tre caselle di controllo selezionate, ma non rinominare il `GetData` metodo `GetSuppliers`. Fare clic su Fine per completare la procedura guidata.


[![Rinominare il metodo GetData per GetSuppliers](working-with-computed-columns-cs/_static/image14.png)](working-with-computed-columns-cs/_static/image13.png)

**Figura 5**: rinominare il `GetData` metodo `GetSuppliers` ([fare clic per visualizzare l'immagine ingrandita](working-with-computed-columns-cs/_static/image15.png))


Al momento di fare clic su Fine, la procedura guidata verrà creare quattro stored procedure e aggiungere il TableAdapter e DataTable corrispondente al set di dati tipizzato.

## <a name="step-4-including-the-computed-column-in-the-tableadapter-s-main-query"></a>Passaggio 4: Inclusa la colonna calcolata nella Query TableAdapter s principale

È ora necessario per l'aggiornamento dell'oggetto TableAdapter e DataTable creato nel passaggio 3 per includere il `FullContactName` colonna calcolata. Questa operazione comporta due passaggi:

1. L'aggiornamento di `Suppliers_Select` stored procedure per restituire il `FullContactName` colonna calcolata, e
2. L'aggiornamento di DataTable per includere un corrispondente `FullContactName` colonna.

Avviare passando a Esplora Server e il drill-down nella cartella Stored procedure. Aprire il `Suppliers_Select` stored procedure e aggiornamento di `SELECT` query per includere il `FullContactName` colonna calcolata:


[!code-sql[Main](working-with-computed-columns-cs/samples/sample4.sql)]

Salvare le modifiche apportate alla stored procedure facendo clic sull'icona Salva nella barra degli strumenti, premendo Ctrl + S o scegliendo Salva `Suppliers_Select` opzione dal menu File.

Successivamente, tornare alla finestra di progettazione set di dati, fare clic su di `SuppliersTableAdapter`e scegliere Configura dal menu di scelta rapida. Si noti che il `Suppliers_Select` colonna include ora il `FullContactName` nella propria raccolta di colonne di dati della colonna.


[![Eseguire la configurazione guidata s TableAdapter per aggiornare le colonne s DataTable](working-with-computed-columns-cs/_static/image17.png)](working-with-computed-columns-cs/_static/image16.png)

**Figura 6**: eseguire i TableAdapter configurazione guidata per aggiornare gli oggetti DataTable colonne ([fare clic per visualizzare l'immagine ingrandita](working-with-computed-columns-cs/_static/image18.png))


Fare clic su Fine per completare la procedura guidata. Verrà aggiunta automaticamente una colonna corrispondente per il `SuppliersDataTable`. La configurazione guidata TableAdapter è abbastanza per rilevare che il `FullContactName` colonna è una colonna calcolata e pertanto in sola lettura. Di conseguenza, imposta la colonna s `ReadOnly` proprietà `true`. Per verificare questa condizione, selezionare la colonna dal `SuppliersDataTable` e quindi passare alla finestra delle proprietà (vedere la figura 7). Si noti che il `FullContactName` colonna s `DataType` e `MaxLength` proprietà vengono inoltre impostate di conseguenza.


[![La colonna FullContactName è contrassegnata come di sola lettura](working-with-computed-columns-cs/_static/image20.png)](working-with-computed-columns-cs/_static/image19.png)

**Figura 7**: il `FullContactName` colonna è contrassegnata come di sola lettura ([fare clic per visualizzare l'immagine ingrandita](working-with-computed-columns-cs/_static/image21.png))


## <a name="step-5-adding-agetsupplierbysupplieridmethod-to-the-tableadapter"></a>Passaggio 5: Aggiunta di un`GetSupplierBySupplierID`metodo all'oggetto TableAdapter

Per questa esercitazione si creerà una pagina ASP.NET che consente di visualizzare i fornitori in una griglia aggiornabile. In oltre esercitazioni sono state aggiornate un singolo record di Business Logic Layer recuperando che un record specifico DAL come DataTable fortemente tipizzato, l'aggiornamento delle relative proprietà e quindi inviarlo DataTable aggiornato il postback al per propagare le modifiche il database. Per eseguire il primo passaggio, il recupero del record da aggiornare DAL - è necessario prima aggiungere un `GetSupplierBySupplierID(supplierID)` DAL metodo.

Fare clic su di `SuppliersTableAdapter` nella struttura del set di dati e scegliere l'opzione Aggiungi Query dal menu di scelta rapida. Come è stato fatto nel passaggio 3, consentire alla procedura guidata genera una nuova stored procedure per noi selezionando l'opzione di creare nuove stored procedure (vedere la figura 3 per una schermata di questo passaggio della procedura guidata). Poiché questo metodo restituirà un record con più colonne, indicano che si desidera utilizzare una query SQL che è un'istruzione SELECT che restituisce righe e fare clic su Avanti.


[![Scegliere l'istruzione SELECT che restituisce righe, opzione](working-with-computed-columns-cs/_static/image23.png)](working-with-computed-columns-cs/_static/image22.png)

**Figura 8**: scegliere l'istruzione SELECT che restituisce righe opzione ([fare clic per visualizzare l'immagine ingrandita](working-with-computed-columns-cs/_static/image24.png))


Nel passaggio successivo richiesto us per la query da utilizzare per questo metodo. Immettere le informazioni seguenti, che restituisce gli stessi campi di dati della query principale, ma per un particolare fornitore.


[!code-sql[Main](working-with-computed-columns-cs/samples/sample5.sql)]

Nella schermata successiva viene chiesto di specificare un nome di stored procedure che verrà generati automaticamente. Nome di questa stored procedure `Suppliers_SelectBySupplierID` e fare clic su Avanti.


[![Nome Suppliers_SelectBySupplierID la Stored Procedure](working-with-computed-columns-cs/_static/image26.png)](working-with-computed-columns-cs/_static/image25.png)

**Figura 9**: nome della Stored Procedure `Suppliers_SelectBySupplierID` ([fare clic per visualizzare l'immagine ingrandita](working-with-computed-columns-cs/_static/image27.png))


Infine, le richieste guidata us per i dati di accedere ai modelli e i nomi di metodo da utilizzare per l'oggetto TableAdapter. Lasciare le caselle di controllo sia selezionate, ma non rinominare il `FillBy` e `GetDataBy` metodi `FillBySupplierID` e `GetSupplierBySupplierID`, rispettivamente.


[![Nome di FillBySupplierID metodi TableAdapter e GetSupplierBySupplierID](working-with-computed-columns-cs/_static/image29.png)](working-with-computed-columns-cs/_static/image28.png)

**Figura 10**: nome i metodi TableAdapter `FillBySupplierID` e `GetSupplierBySupplierID` ([fare clic per visualizzare l'immagine ingrandita](working-with-computed-columns-cs/_static/image30.png))


Fare clic su Fine per completare la procedura guidata.

## <a name="step-6-creating-the-business-logic-layer"></a>Passaggio 6: La creazione del livello di logica di Business

Prima di creare una pagina ASP.NET che usa la colonna calcolata creata nel passaggio 1, è innanzitutto necessario aggiungere i metodi corrispondenti nel BLL. La pagina ASP.NET, che verrà creata nel passaggio 7, consente agli utenti di visualizzare e modificare i fornitori. Pertanto, è necessario il livello Business LOGIC per fornire almeno un metodo per ottenere tutti i fornitori e un altro per l'aggiornamento di un particolare fornitore.

Creare un nuovo file di classe denominato `SuppliersBLLWithSprocs` nel `~/App_Code/BLL` cartella e aggiungere il codice seguente:


[!code-csharp[Main](working-with-computed-columns-cs/samples/sample6.cs)]

Come le altre classi BLL, `SuppliersBLLWithSprocs` ha un `protected` `Adapter` proprietà che restituisce un'istanza di `SuppliersTableAdapter` classe insieme a due `public` metodi: `GetSuppliers` e `UpdateSupplier`. Il `GetSuppliers` metodo chiama e restituisce il `SuppliersDataTable` restituito dal corrispondente `GetSupplier` metodo nel livello di accesso ai dati. Il `UpdateSupplier` recupera le informazioni sull'aggiornamento tramite una chiamata a s DAL fornitore particolare `GetSupplierBySupplierID(supplierID)` metodo. Viene quindi aggiornato il `CategoryName`, `ContactName`, e `ContactTitle` proprietà ed esegue il commit di queste modifiche al database chiamando il livello di accesso ai dati di s `Update` metodo modificati passando `SuppliersRow` oggetto.

> [!NOTE]
> Ad eccezione di `SupplierID` e `CompanyName`, consentono tutte le colonne nella tabella Suppliers `NULL` valori. Pertanto, se passato `contactName` o `contactTitle` i parametri sono `null` è necessario impostare corrispondente `ContactName` e `ContactTitle` proprietà per un `NULL` database valore utilizzando il `SetContactNameNull` e `SetContactTitleNull`metodi, rispettivamente.


## <a name="step-7-working-with-the-computed-column-from-the-presentation-layer"></a>Passaggio 7: Utilizzo con la colonna calcolata dal livello di presentazione

Con la colonna calcolata aggiunta a di `Suppliers` tabella e DAL e BLL aggiornati di conseguenza, è possibile compilare una pagina ASP.NET che funziona con il `FullContactName` colonna calcolata. Aprire il `ComputedColumns.aspx` nella pagina di `AdvancedDAL` cartelle e trascinare un controllo GridView dalla casella degli strumenti nella finestra di progettazione. Impostare i dispositivi di GridView `ID` proprietà `Suppliers` e dal suo smart tag, associarlo a un nuovo oggetto ObjectDataSource denominato `SuppliersDataSource`. Configurare ObjectDataSource per utilizzare il `SuppliersBLLWithSprocs` classe è stato aggiunto il passaggio 6 e fare clic su Avanti.


[![Configurare ObjectDataSource per utilizzare la classe SuppliersBLLWithSprocs](working-with-computed-columns-cs/_static/image32.png)](working-with-computed-columns-cs/_static/image31.png)

**Figura 11**: configurare ObjectDataSource per utilizzare il `SuppliersBLLWithSprocs` classe ([fare clic per visualizzare l'immagine ingrandita](working-with-computed-columns-cs/_static/image33.png))


Sono disponibili solo due metodi definiti nel `SuppliersBLLWithSprocs` classe: `GetSuppliers` e `UpdateSupplier`. Assicurarsi che questi due metodi sono specificati nella finestra selezione aggiornare schede, rispettivamente e fare clic su Fine per completare la configurazione di ObjectDataSource.

Dopo aver completato la configurazione guidata origine dati, Visual Studio aggiungerà un BoundField per ognuno dei campi di dati restituiti. Rimuovere il `SupplierID` BoundField e modificare il `HeaderText` le proprietà del `CompanyName`, `ContactName`, `ContactTitle`, e `FullContactName` BoundField a società, Contact Name, titolo e nome completo di contatto, rispettivamente. Dalla smart tag, selezionare la casella di controllo Abilita modifica per attivare le funzionalità di modifica predefinite di GridView s.

Oltre ad aggiungere BoundField a GridView, completamento della creazione guidata origine dati comporta anche Visual Studio consente di impostare il s ObjectDataSource `OldValuesParameterFormatString` proprietà originale\_{0}. Ripristinare questa impostazione il valore predefinito, {0}.

Dopo aver apportato queste modifiche a GridView e ObjectDataSource, il markup dichiarativo dovrebbe essere simile al seguente:


[!code-aspx[Main](working-with-computed-columns-cs/samples/sample7.aspx)]

Successivamente, visitare la pagina tramite un browser. Come mostrato nella figura 12, ogni fornitore è elencato in una griglia che include il `FullContactName` colonna, il cui valore è semplicemente la concatenazione delle altre tre colonne formattata come `ContactName` (`ContactTitle`, `CompanyName`).


[![Ogni fornitore è elencato nella griglia](working-with-computed-columns-cs/_static/image35.png)](working-with-computed-columns-cs/_static/image34.png)

**Figura 12**: ogni fornitore è elencato nella griglia ([fare clic per visualizzare l'immagine ingrandita](working-with-computed-columns-cs/_static/image36.png))


Fare clic sul pulsante Modifica per un particolare fornitore provoca un postback e che la riga è stato eseguito nel relativo tipo di modifica dell'interfaccia (vedere Figura 13). Le prime tre colonne di cui è stato eseguito il rendering in base all'impostazione predefinita la modifica di interfaccia: controllo di una casella di testo la cui `Text` proprietà è impostata sul valore del campo dati. Il `FullContactName` colonna, tuttavia, rimane come testo. Quando il BoundField sono stati aggiunti al controllo GridView. dopo aver completato la configurazione guidata origine dati, il `FullContactName` BoundField s `ReadOnly` è stata impostata su `true` perché corrispondente `FullContactName` colonna il `SuppliersDataTable` è il relativo `ReadOnly` proprietà impostata su `true`. Come indicato nel passaggio 4, il `FullContactName` s `ReadOnly` è stata impostata su `true` perché TableAdapter rilevato che la colonna è una colonna calcolata.


[![La colonna FullContactName non è modificabile](working-with-computed-columns-cs/_static/image38.png)](working-with-computed-columns-cs/_static/image37.png)

**Figura 13**: il `FullContactName` colonna non è modificabile ([fare clic per visualizzare l'immagine ingrandita](working-with-computed-columns-cs/_static/image39.png))


Vado avanti e aggiornare il valore di uno o più colonne modificabili e fare clic su Aggiorna. Si noti come `FullContactName` s valore viene aggiornato automaticamente per riflettere la modifica.

> [!NOTE]
> GridView utilizza attualmente BoundField per i campi modificabili, pertanto il valore predefinito di interfaccia di modifica. Poiché il `CompanyName` campo è obbligatorio, deve essere convertito in un TemplateField che include un RequiredFieldValidator. Lasciare questo campo come esercizio per il lettore di interesse. Consultare la [aggiunta di controlli di convalida per la modifica e inserimento di interfacce](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) esercitazione per istruzioni dettagliate sulla conversione di un BoundField in un TemplateField e aggiunta di controlli di convalida.


## <a name="summary"></a>Riepilogo

Quando si definisce lo schema per una tabella, Microsoft SQL Server consente l'inclusione di colonne calcolate. Si tratta di colonne i cui valori sono calcolati da un'espressione che in genere fa riferimento ai valori di altre colonne dello stesso record. Poiché i valori per le colonne calcolate sono basate su un'espressione, sono di sola lettura e non può essere assegnati un valore in un `INSERT` o `UPDATE` istruzione. Ciò implica problemi quando si utilizza una colonna calcolata nella query principale di un oggetto TableAdapter che tenta di generare automaticamente corrispondente `INSERT`, `UPDATE`, e `DELETE` istruzioni.

In questa esercitazione sono descritte le tecniche per aggirare i problemi derivanti dalle colonne calcolate. In particolare, le stored procedure viene usata nel nostro TableAdapter per superare il TableAdapter che Usa istruzioni SQL ad hoc inerenti la vulnerabilità. Per avere la configurazione guidata TableAdapter che crea nuove stored procedure, è importante che abbiamo la query principale inizialmente omettere le colonne calcolate perché la loro presenza impedisce la generazione le procedure di modifica archiviata dati. Dopo aver configurato inizialmente TableAdapter, il relativo `SelectCommand` stored procedure può essere retooled per includere le colonne calcolate.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Lead revisori per questa esercitazione sono stati Geisenow Hilton e Teresa Murphy. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Precedente](adding-additional-datatable-columns-cs.md)
[Successivo](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs.md)
