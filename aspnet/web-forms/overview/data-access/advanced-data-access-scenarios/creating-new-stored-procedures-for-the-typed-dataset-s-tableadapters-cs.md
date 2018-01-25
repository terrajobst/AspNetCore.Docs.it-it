---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs
title: Creazione di nuove Stored procedure per gli oggetti TableAdapter del DataSet tipizzato (c#) | Documenti Microsoft
author: rick-anderson
description: Nelle esercitazioni precedenti abbiamo istruzioni SQL create nel codice e passare le istruzioni per il database deve essere eseguito. Un approccio alternativo consiste nell'utilizzare s...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: 751282ca-5870-4d66-84e4-6cefae23eb4a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs
msc.type: authoredcontent
ms.openlocfilehash: 715f7e3fae89e773b686faa7c49522c587693eec
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="creating-new-stored-procedures-for-the-typed-datasets-tableadapters-c"></a>Creazione di nuove Stored procedure per gli oggetti TableAdapter del DataSet tipizzato (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare codice](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_67_CS.zip) o [Scarica il PDF](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/datatutorial67cs1.pdf)

> Nelle esercitazioni precedenti abbiamo istruzioni SQL create nel codice e passare le istruzioni per il database deve essere eseguito. Un'alternativa consiste nell'utilizzare le stored procedure, in cui le istruzioni SQL predefinite presenti nel database. In questa esercitazione è come disporre la configurazione guidata TableAdapter generate nuove stored procedure per noi.


## <a name="introduction"></a>Introduzione

Livello accesso dati (DAL) per queste esercitazioni utilizza dataset tipizzati. Come descritto nel [la creazione di un livello di accesso ai dati](../introduction/creating-a-data-access-layer-cs.md) esercitazione, i set di dati tipizzato sono costituiti da DataTable fortemente tipizzato e gli oggetti TableAdapter. DataTable rappresentano entità logiche nel sistema durante l'interfaccia TableAdapter con il database sottostante per eseguire le operazioni di accesso ai dati. Ciò include popolamento DataTable con i dati, l'esecuzione di query che restituiscono dati scalari, e inserimento, aggiornamento ed eliminazione di record dal database.

I comandi SQL eseguiti dagli oggetti TableAdapter possono essere entrambe le istruzioni SQL ad hoc, ad esempio `SELECT columnList FROM TableName`, o le stored procedure. Gli oggetti TableAdapter nell'architettura utilizzare istruzioni SQL ad hoc. Molti sviluppatori e amministratori di database, tuttavia, preferiscano le stored procedure le istruzioni SQL ad hoc per motivi di sicurezza, gestibilità e aggiornabilità. Altri preferiscono ardently istruzioni SQL ad hoc per loro flessibilità. Il proprio lavoro si prediligono le stored procedure su istruzioni SQL ad hoc, ma ha scelto di utilizzare istruzioni SQL ad hoc per semplificare le esercitazioni precedenti.

Quando la definizione di un oggetto TableAdapter o aggiunta di nuovi metodi, la configurazione guidata TableAdapter s rende altrettanto facile creare nuove stored procedure o utilizzare le stored procedure esistente come avviene per l'utilizzo di istruzioni SQL ad hoc. In questa esercitazione si esamineranno come disporre la configurazione guidata TableAdapter s generazione automatica delle stored procedure. Nella prossima esercitazione verranno esaminati come configurare i metodi di s TableAdapter per utilizzare le stored procedure esistente o creati manualmente.

> [!NOTE]
> Vedere il post di blog di Rob Howard [non ancora procedure di archiviati di utilizzo?](http://grokable.com/2003/11/dont-use-stored-procedures-yet-must-be-suffering-from-nihs-not-invented-here-syndrome/) e [Frans Bouma](https://weblogs.asp.net/fbouma/) post di blog s [Stored procedure sono danneggiati, Kay M?](https://weblogs.asp.net/fbouma/archive/2003/11/18/38178.aspx) per un dibattito brillante sui vantaggi e svantaggi delle stored procedure e SQL ad hoc.


## <a name="stored-procedure-basics"></a>Nozioni fondamentali sulla stored Procedure

Le funzioni sono un costrutto comune a tutti i linguaggi di programmazione. Una funzione è una raccolta di istruzioni che vengono eseguite quando viene chiamata la funzione. Le funzioni possono accettare parametri di input e, facoltativamente, possono restituire un valore. *[Stored procedure](http://en.wikipedia.org/wiki/Stored_procedure)*  sono costrutti di database che condividono molte analogie con le funzioni nei linguaggi di programmazione. Una stored procedure è costituita da un set di istruzioni T-SQL che vengono eseguite quando viene chiamata la stored procedure. Una stored procedure può accettare zero a molti parametri di input e può restituire valori scalari, i parametri di output, o, in genere, set di risultati da `SELECT` query.

> [!NOTE]
> Stored procedure sono spesso detta stored procedure o SP.


Vengono create stored procedure utilizzando il [ `CREATE PROCEDURE` ](https://msdn.microsoft.com/library/aa258259(SQL.80).aspx) istruzione T-SQL. Ad esempio, lo script T-SQL seguente crea una stored procedure denominata `GetProductsByCategoryID` che accetta un singolo parametro denominato `@CategoryID` e restituisce il `ProductID`, `ProductName`, `UnitPrice`, e `Discontinued` i campi di tali colonne nel `Products` tabella che dispone di un corrispondente `CategoryID` valore:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample1.sql)]

Dopo aver creata questa stored procedure, può essere chiamato utilizzando la sintassi seguente:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample2.sql)]

> [!NOTE]
> Nella prossima esercitazione verrà esaminato la creazione di stored procedure tramite l'IDE di Visual Studio. Per questa esercitazione, tuttavia, verrà generato automaticamente le stored procedure per la configurazione guidata TableAdapter.


Oltre a restituire semplicemente i dati, le stored procedure vengono spesso utilizzate per eseguire più comandi di database all'interno dell'ambito di una singola transazione. Una stored procedure denominata `DeleteCategory`, ad esempio, potrebbe richiedere un `@CategoryID` parametro ed eseguire due `DELETE` istruzioni: prima di tutto, uno per eliminare i prodotti correlati e l'altro eliminazione categoria specificata. Più istruzioni all'interno di una stored procedure sono *non* automaticamente sottoposta a wrapping in una transazione. Altri comandi T-SQL necessario essere emesso per garantire la stored procedure s che più comandi vengono considerati come operazione atomica. Vedremo come eseguire il wrapping di comandi una stored procedure all'interno dell'ambito di una transazione nell'esercitazione successiva.

Quando si utilizzano stored procedure all'interno di un'architettura, il livello di accesso ai dati s richiama una stored procedure, anziché eseguire un'istruzione SQL ad hoc. Consente di centralizzare la posizione delle istruzioni SQL eseguite (on database) anziché averlo definito all'interno dell'architettura di applicazione s. Questa centralizzazione senza dubbio rende più semplice trovare, analizzare e ottimizzare le query e fornisce un'indicazione più completa da dove e come il database è in uso.

Per ulteriori informazioni sulle nozioni fondamentali sulla stored procedure, consultare le risorse nella sezione di approfondimento alla fine di questa esercitazione.

## <a name="step-1-creating-the-advanced-data-access-layer-scenarios-web-pages"></a>Passaggio 1: Creazione di pagine Web scenari di livello accesso dati avanzate

Prima di iniziare la discussione sulla creazione di un utilizzo di stored procedure DAL, consentire s prima richiedere alcuni minuti per creare le pagine ASP.NET nel progetto sito Web che è necessario per questo e per le esercitazioni più avanti. Per iniziare, aggiungere una nuova cartella denominata `AdvancedDAL`. Successivamente, aggiungere le seguenti pagine ASP.NET a quella cartella, assicurandosi di associare ogni pagina con il `Site.master` pagina master:

- `Default.aspx`
- `NewSprocs.aspx`
- `ExistingSprocs.aspx`
- `JOINs.aspx`
- `AddingColumns.aspx`
- `ComputedColumns.aspx`
- `EncryptingConfigSections.aspx`
- `ManagedFunctionsAndSprocs.aspx`


![Aggiungere le pagine ASP.NET per le esercitazioni di scenari livello accesso dati avanzati](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image1.png)

**Figura 1**: aggiungere le pagine ASP.NET per le esercitazioni di scenari livello accesso dati avanzati


Come in altre cartelle, `Default.aspx` nel `AdvancedDAL` cartella elencherà le esercitazioni nella relativa sezione. Tenere presente che il `SectionLevelTutorialListing.ascx` controllo utente fornisce questa funzionalità. Pertanto, aggiungere il controllo utente `Default.aspx` trascinandolo da Esplora soluzioni nella pagina di visualizzazione della struttura s.


[![Aggiungere il controllo utente SectionLevelTutorialListing.ascx Default.aspx](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image3.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image2.png)

**Figura 2**: aggiungere il `SectionLevelTutorialListing.ascx` controllo utente in `Default.aspx` ([fare clic per visualizzare l'immagine ingrandita](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image4.png))


Infine, aggiungere tali pagine come voci per il `Web.sitemap` file. In particolare, aggiungere il markup seguente dopo l'utilizzo dei dati in batch `<siteMapNode>`:


[!code-xml[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample3.xml)]

Dopo aver aggiornato `Web.sitemap`, dedicare alcuni minuti per visualizzare il sito Web esercitazioni tramite un browser. Il menu a sinistra ora include gli elementi per le esercitazioni di scenari DAL avanzate.


![Mappa del sito include ora le voci per le esercitazioni di scenari avanzati di DAL](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image5.png)

**Figura 3**: mappa del sito include ora le voci per le esercitazioni di scenari avanzati di DAL


## <a name="step-2-configuring-a-tableadapter-to-create-new-stored-procedures"></a>Passaggio 2: Configurazione di un oggetto TableAdapter per creare nuove Stored procedure

Per illustrare la creazione di un livello di accesso ai dati che utilizza le stored procedure anziché istruzioni SQL ad hoc, s consentono di creare un nuovo DataSet tipizzati nel `~/App_Code/DAL` cartella denominata `NorthwindWithSprocs.xsd`. Poiché è stato eseguito l'accesso tramite questo processo descritto in dettaglio nelle esercitazioni precedenti, si procederà rapidamente i passaggi qui. Se si improvvisi o necessario ulteriori istruzioni dettagliate nella creazione e configurazione di un DataSet tipizzato, si riferiscono nuovamente al [la creazione di un livello di accesso ai dati](../introduction/creating-a-data-access-layer-cs.md) esercitazione.

Aggiungere un nuovo set di dati al progetto facendo clic su di `DAL` cartella, scegliere Aggiungi nuovo elemento e selezionando il modello di set di dati, come illustrato nella figura 4.


[![Aggiungere un nuovo set di dati tipizzato al progetto denominato NorthwindWithSprocs.xsd](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image7.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image6.png)

**Figura 4**: aggiungere un nuovo set di dati tipizzato per il progetto denominato `NorthwindWithSprocs.xsd` ([fare clic per visualizzare l'immagine ingrandita](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image8.png))


Questo verrà creare il nuovo set di dati tipizzato, aprire la finestra di progettazione, creare un nuovo TableAdapter e avviare la configurazione guidata TableAdapter. Innanzitutto la configurazione guidata TableAdapter s chiesto di selezionare il database da utilizzare. La stringa di connessione al database Northwind dovrebbe essere elencata nell'elenco a discesa. Selezionare questa opzione e fare clic su Avanti.

In questa schermata successiva è possibile scegliere come TableAdapter deve accedere al database. Nelle esercitazioni precedenti, è selezionata la prima opzione, Usa istruzioni SQL. Per questa esercitazione, selezionare la seconda opzione, creare nuove stored procedure e fare clic su Avanti.


[![Indicare TableAdpater per creare nuove Stored procedure](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image10.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image9.png)

**Figura 5**: indicare il TableAdpater per creare nuove Stored procedure ([fare clic per visualizzare l'immagine ingrandita](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image11.png))


Proprio come con le istruzioni SQL ad hoc, nel passaggio seguente viene chiesto di fornire il `SELECT` istruzione per la query principale s di TableAdapter. Ma anziché il `SELECT` istruzione immesso per eseguire direttamente una query ad hoc, la configurazione guidata TableAdapter s creerà una stored procedure che contiene questo `SELECT` query.

Utilizzare la seguente `SELECT` query per questo oggetto TableAdapter:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample4.sql)]


[![Immettere la Query SELECT](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image13.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image12.png)

**Figura 6**: immettere il `SELECT` Query ([fare clic per visualizzare l'immagine ingrandita](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image14.png))


> [!NOTE]
> La query precedente è leggermente diverso dalla query principale di `ProductsTableAdapter` nel `Northwind` DataSet tipizzato. Tenere presente che il `ProductsTableAdapter` nel `Northwind` DataSet tipizzata include due sottoquery correlate per riportare il nome di categoria e il nome della società per ogni categoria di prodotto s e fornitore. Nella [l'aggiornamento dell'oggetto TableAdapter per usare join](updating-the-tableadapter-to-use-joins-cs.md) verranno esaminati l'aggiunta di questa esercitazione i dati relativi a questo oggetto TableAdapter.


È opportuno fare clic sul pulsante Opzioni avanzate. Da qui è possibile specificare se la procedura guidata deve generare anche le istruzioni delete, update e insert per TableAdapter, se si desidera usare la concorrenza ottimistica e indica se la tabella di dati deve essere aggiornata dopo inserimenti e aggiornamenti. L'opzione di istruzioni genera istruzioni Insert, Update e Delete è selezionata per impostazione predefinita. Lasciare selezionata. Per questa esercitazione, lasciare deselezionata l'Usa le opzioni di concorrenza ottimistica.

Quando si verificano le stored procedure create automaticamente la configurazione guidata TableAdapter, sembra che l'aggiornamento, l'opzione di tabella di dati viene ignorato. Indipendentemente dal fatto che sia selezionata la casella di controllo risultante insert e update stored procedure recuperano il record appena inserite o aggiornate appena, come verrà illustrato nel passaggio 3.


![Lasciare le istruzioni genera istruzioni Insert, Update e Delete opzione selezionata](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image15.png)

**Figura 7**: lasciare le istruzioni genera istruzioni Insert, Update e Delete opzione selezionata


> [!NOTE]
> Se è selezionata l'opzione Usa concorrenza ottimistica, la procedura guidata aggiungerà le condizioni aggiuntive per il `WHERE` clausola che impediscono l'aggiornamento se non vi sono modifiche in altri campi di dati. Fare riferimento al [implementazione di concorrenza ottimistica](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md) esercitazione per ulteriori informazioni sull'uso della funzionalità di controllo della concorrenza ottimistica predefinita s TableAdapter.


Dopo aver immesso il `SELECT` query e per confermare che sia selezionata l'opzione genera istruzioni Insert, Update e Delete di istruzioni, fare clic su Avanti. Questa schermata successiva, illustrata nella figura 8, richiesto per i nomi delle stored procedure che creerà la procedura guidata per la selezione, inserimento, aggiornamento ed eliminazione dei dati. Modificare questi nomi procedure di archiviazione `Products_Select`, `Products_Insert`, `Products_Update`, e `Products_Delete`.


[![Rinominare le Stored procedure](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image17.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image16.png)

**Figura 8**: rinominare una Stored procedure ([fare clic per visualizzare l'immagine ingrandita](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image18.png))


Per visualizzare la configurazione guidata TableAdapter verrà utilizzato per creare quattro stored procedure T-SQL, fare clic sul pulsante Anteprima Script SQL. Nella finestra di dialogo Anteprima Script SQL è possibile salvare lo script in un file o copiarlo negli Appunti.


![Visualizzare in anteprima lo Script SQL utilizzato per generare le Stored procedure](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image19.png)

**Figura 9**: anteprima Script SQL utilizzato per generare le Stored procedure


Dopo la denominazione delle stored procedure, fare clic su Avanti metodi corrispondenti ai TableAdapter di nome. Proprio come quando si Usa istruzioni SQL ad hoc, è possibile creare metodi che Riempi un DataTable esistente o restituiscono uno nuovo. È inoltre possibile specificare se l'oggetto TableAdapter deve includere il modello di DB-Direct per l'inserimento, aggiornamento ed eliminazione di record. Lasciare tutti i tre caselle di controllo selezionate, ma rinominare la restituzione di un metodo di DataTable per `GetProducts` (come illustrato nella figura 10).


[![Denominare i metodi di riempimento e GetProducts](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image21.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image20.png)

**Figura 10**: i metodi di nome `Fill` e `GetProducts` ([fare clic per visualizzare l'immagine ingrandita](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image22.png))


Fare clic su Avanti per visualizzare un riepilogo dei passaggi che verrà eseguita la procedura guidata. Completare la procedura guidata facendo clic sul pulsante Fine. Dopo aver completato la procedura guidata, verrà restituito per i DataSet di progettazione, che dovrebbe ora includere la `ProductsDataTable`.


[![Progettazione DataSet s Mostra il ProductsDataTable appena aggiunta](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image24.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image23.png)

**Figura 11**: s set di dati di progettazione Mostra appena aggiunto `ProductsDataTable` ([fare clic per visualizzare l'immagine ingrandita](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image25.png))


## <a name="step-3-examining-the-newly-created-stored-procedures"></a>Passaggio 3: Esaminare le Stored procedure appena create

La configurazione guidata TableAdapter utilizzata nel passaggio 2 automaticamente create le stored procedure per la selezione, inserimento, aggiornamento ed eliminazione dei dati. Queste stored procedure possono essere visualizzate o modificate tramite Visual Studio in Esplora Server e il drill-down nella cartella database s Stored procedure. Come mostrato nella figura 12, il database di Northwind contiene quattro nuove stored procedure: `Products_Delete`, `Products_Insert`, `Products_Select`, e `Products_Update`.


![Le quattro Stored procedure create nel passaggio 2 sono reperibile nella cartella di Stored procedure del Database s](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image26.png)

**Figura 12**: quattro Stored procedure create nel passaggio 2 sono reperibile nella cartella di Stored procedure del Database s


> [!NOTE]
> Se non è possibile visualizzare Esplora Server, passare al menu Visualizza e selezionare l'opzione di Esplora Server. Se non vi sono relativi al prodotto stored procedure aggiunte dal passaggio 2, provare a facendo clic sulla cartella Stored procedure e scegliendo Aggiorna.


Per visualizzare o modificare una stored procedure, fare doppio clic sul relativo nome in Esplora Server oppure in alternativa, fare clic su stored procedure e scegliere Apri. Figura 13 illustra il `Products_Delete` stored procedure, di apertura.


[![Stored procedure possono essere aperto e modificate da Visual Studio](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image28.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image27.png)

**Figura 13**: Stored procedure possono essere aperti e modificati da all'interno di Visual Studio ([fare clic per visualizzare l'immagine ingrandita](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image29.png))


I contenuti di entrambe le `Products_Delete` e `Products_Select` stored procedure sono piuttosto semplice. Il `Products_Insert` e `Products_Update` stored procedure, d'altra parte, garantiscono un attentamente come entrambe eseguono un `SELECT` istruzione dopo le `INSERT` e `UPDATE` istruzioni. Ad esempio, l'istruzione SQL seguente costituisce il `Products_Insert` stored procedure:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample5.sql)]

La stored procedure accetta come parametri di input di `Products` colonne restituite dal `SELECT` query specificata in cui la configurazione guidata TableAdapter s e questi valori vengono utilizzati in un `INSERT` istruzione. Dopo il `INSERT` istruzione, un `SELECT` query viene utilizzata per restituire il `Products` i valori di colonna (incluso il `ProductID`) del record appena aggiunto. Questa funzionalità di aggiornamento è utile quando si aggiunge un nuovo record utilizzando il modello di aggiornamento Batch come automaticamente gli aggiornamenti appena aggiunta `ProductRow` istanze `ProductID` con i valori a incremento automatico assegnati dal database.

Il codice seguente viene illustrata questa funzionalità. Contiene un `ProductsTableAdapter` e `ProductsDataTable` creato per il `NorthwindWithSprocs` DataSet tipizzato. Un nuovo prodotto viene aggiunto al database tramite la creazione di un `ProductsRow` istanza, fornendo i relativi valori e chiamando i TableAdapter `Update` metodo passando la `ProductsDataTable`. Internamente, i TableAdapter `Update` metodo enumera il `ProductsRow` istanze nel DataTable passato (in questo esempio è presente solo uno, uno è stato appena aggiunto) ed esegue appropriata di inserire, aggiornare o eliminare comandi. In questo caso, il `Products_Insert` stored procedure viene eseguita, che aggiunge un nuovo record per il `Products` tabella e restituisce i dettagli del record appena aggiunti. Il `ProductsRow` istanza s `ProductID` valore viene quindi aggiornato. Dopo il `Update` completamento del metodo, è possibile accedere al record appena aggiunti s `ProductID` valore tramite il `ProductsRow` s `ProductID` proprietà.


[!code-csharp[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample6.cs)]

Il `Products_Update` stored procedure allo stesso modo include un `SELECT` istruzione dopo il relativo `UPDATE` istruzione.


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample7.sql)]

Si noti che questa stored procedure includono due parametri di input per `ProductID`: `@Original_ProductID` e `@ProductID`. Questa funzionalità consente di scenari in cui la chiave primaria può essere modificata. Ad esempio, in un database di dipendenti, ogni record dipendente può usare il numero di previdenza sociale s dipendente come la propria chiave primaria. Per modificare un numero di previdenza sociale di s dipendente esistente, è necessario specificare sia il nuovo numero di previdenza sociale e quella originale. Per il `Products` tabella, tale funzionalità non è necessaria perché il `ProductID` colonna è un `IDENTITY` colonna e non deve mai essere modificato. In realtà, il `UPDATE` istruzione il `Products_Update` t di stored procedure includono il `ProductID` colonna nell'elenco di colonne. In tal caso, mentre `@Original_ProductID` viene utilizzata per il `UPDATE` istruzione s `WHERE` clausola è superflua per il `Products` tabella e potrebbe essere sostituita con il `@ProductID` parametro. Quando si modifica un parametri delle stored procedure s è importante che i metodi TableAdapter che usano la stored procedure vengono inoltre aggiornati.

## <a name="step-4-modifying-a-stored-procedure-s-parameters-and-updating-the-tableadapter"></a>Passaggio 4: Modifica dei parametri s una Stored Procedure e l'aggiornamento dell'oggetto TableAdapter

Poiché il `@Original_ProductID` parametro è superflua, s consentono di rimuovere il messaggio di `Products_Update` stored procedure di tutto. Aprire il `Products_Update` stored procedure elimina il `@Original_ProductID` parametro e, nel `WHERE` clausola del `UPDATE` istruzione, cambiare il nome del parametro utilizzato da `@Original_ProductID` per `@ProductID`. Dopo aver apportato queste modifiche, il codice T-SQL all'interno della stored procedure dovrebbe essere simile al seguente:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample8.sql)]

Per salvare le modifiche apportate al database, fare clic sull'icona Salva nella barra degli strumenti o premere Ctrl + S. A questo punto, il `Products_Update` stored procedure non è previsto un `@Original_ProductID` parametro di input, ma il TableAdapter è configurato per passare un parametro di questo tipo. È possibile visualizzare i parametri dell'oggetto TableAdapter verrà inviato al `Products_Update` selezionando il TableAdapter in Progettazione DataSet, passare alla finestra proprietà e facendo clic sui puntini di sospensione nella stored procedure di `UpdateCommand` s `Parameters` insieme. Verrà visualizzata la finestra di dialogo Editor della raccolta Parameters illustrata nella figura 14.


![Gli elenchi di Editor della raccolta di parametri i parametri utilizzati passato per il Products_Update Stored Procedure](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image30.png)

**Nella figura 14**: passato gli elenchi di Editor della raccolta di parametri i parametri utilizzati per il `Products_Update` Stored Procedure


È possibile rimuovere questo parametro da qui, selezionare semplicemente il `@Original_ProductID` parametro dall'elenco dei membri e fare clic sul pulsante Rimuovi.

In alternativa, è possibile aggiornare i parametri utilizzati per tutti i metodi facendo clic sul TableAdapter nella finestra di progettazione e scegliendo Configura. Verrà visualizzata la configurazione guidata TableAdapter, elenca le stored procedure utilizzate per la selezione, inserimento, aggiornamento, e l'eliminazione, insieme ai parametri di stored procedure che prevede di ricevere. Se si fa clic nell'elenco a discesa di aggiornamenti è possibile visualizzare il `Products_Update` stored procedure prevede parametri di input, che ora non include più `@Original_ProductID` (vedere Figura 15). Fare clic su Fine per aggiornare automaticamente la raccolta di parametri utilizzata dal TableAdapter.


[![In alternativa, è possibile utilizzare la configurazione guidata TableAdapter s per le raccolte di parametri di metodi di aggiornamento](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image32.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image31.png)

**Figura 15**: in alternativa, è possibile utilizzare i TableAdapter configurazione guidata per aggiornare il metodi parametro raccolte ([fare clic per visualizzare l'immagine ingrandita](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image33.png))


## <a name="step-5-adding-additional-tableadapter-methods"></a>Passaggio 5: Aggiunta di metodi TableAdapter aggiuntive

Come passaggio 2 illustrato, quando si crea un nuovo TableAdapter è facile avere le stored procedure corrispondente è state generate automaticamente. Lo stesso vale quando si aggiungono metodi aggiuntivi per un oggetto TableAdapter. Per illustrare questo concetto, consentire s di aggiungere un `GetProductByProductID(productID)` metodo il `ProductsTableAdapter` creato nel passaggio 2. Questo metodo avrà come input un `ProductID` valore e restituire i dettagli relativi al prodotto specificato.

Avviare facendo clic sul TableAdapter e scegliendo Aggiungi Query dal menu di scelta rapida.


![Aggiungere una nuova Query al TableAdapter](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image34.png)

**Figura 16**: aggiungere una nuova Query al TableAdapter


Verrà avviata la configurazione guidata Query TableAdapter, quale innanzitutto richiesto per la modalità dell'oggetto TableAdapter deve accedere al database. Per una nuova stored procedure creata, scegliere Crea una nuova opzione di stored procedure e fare clic su Avanti.


[![Scegliere la creazione di una nuova stored procedure, opzione](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image36.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image35.png)

**Figura 17**: scegliere di creare una nuova stored procedure, opzione ([fare clic per visualizzare l'immagine ingrandita](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image37.png))


Nella schermata successiva viene chiesto di specificare per identificare il tipo di query da eseguire, verrà restituito un set di righe o di un singolo valore scalare o eseguire un `UPDATE`, `INSERT`, o `DELETE` istruzione. Poiché il `GetProductByProductID(productID)` metodo verrà restituita una riga, lasciare l'istruzione SELECT che restituisce l'opzione di riga è selezionata e scegliere "Avanti".


[![Scegliere l'istruzione SELECT che restituisce righe, opzione](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image39.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image38.png)

**Figura 18**: scegliere l'istruzione SELECT che restituisce righe opzione ([fare clic per visualizzare l'immagine ingrandita](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image40.png))


Nella schermata successiva viene visualizzata la query principale di s TableAdapter, contenente solo il nome della stored procedure (`dbo.Products_Select`). Sostituire il nome della stored procedure con il seguente `SELECT` istruzione che restituisce tutti i campi di prodotto per un prodotto specifico:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample9.sql)]


[![Sostituire il nome di Stored Procedure con una Query di selezione](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image42.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image41.png)

**Figura 19**: sostituire il nome della Stored Procedure con un `SELECT` Query ([fare clic per visualizzare l'immagine ingrandita](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image43.png))


La schermata successiva viene chiesto di denominare la stored procedure che verrà creata. Immettere il nome `Products_SelectByProductID` e fare clic su Avanti.


[![Denominare il nuovo Products_SelectByProductID Stored Procedure](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image45.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image44.png)

**Figura 20**: nome della nuova Stored Procedure `Products_SelectByProductID` ([fare clic per visualizzare l'immagine ingrandita](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image46.png))


Il passaggio finale della procedura guidata consente di modificare il metodo nomi generati, nonché indicano se si desidera utilizzare il riempimento di un modello di DataTable, restituire un modello di DataTable o entrambi. Per questo metodo, non entrambe le opzioni selezionate, ma i metodi da rinominare `FillByProductID` e `GetProductByProductID`. Fare clic su Avanti per visualizzare un riepilogo dei passaggi eseguirà la procedura guidata, quindi fare clic su Fine per completare la procedura guidata.


[![Rinominare i metodi s TableAdapter FillByProductID e GetProductByProductID](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image48.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image47.png)

**Figura 21**: rinominare i metodi TableAdapter s `FillByProductID` e `GetProductByProductID` ([fare clic per visualizzare l'immagine ingrandita](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image49.png))


Dopo aver completato la procedura guidata, TableAdapter dispone di un metodo di nuovo disponibile, `GetProductByProductID(productID)` che, quando richiamata, eseguirà il `Products_SelectByProductID` stored procedure che è stato appena creata. Dedicare alcuni minuti per visualizzare questa nuova stored procedure da Esplora Server drill-down della cartella Stored Procedures e aprendo `Products_SelectByProductID` (se non è presente, fare doppio clic sulla cartella Stored procedure e scegliere Aggiorna).

Si noti che il `SelectByProductID` stored procedure accetta `@ProductID` come parametro di input ed esegue il `SELECT` istruzione immesso nella procedura guidata.


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample10.sql)]

## <a name="step-6-creating-a-business-logic-layer-class"></a>Passaggio 6: Creazione di una classe di livello della logica di Business

In tutta la serie di esercitazioni è stato strived mantenere un'architettura a più livelli in cui il livello di presentazione apportate tutte le chiamate al livello BLL (Business LOGIC Layer). Per essere conformi a questa decisione progettuale, è necessario innanzitutto creare una classe BLL per il nuovo set di dati tipizzato prima che sia possibile accedere a dati del prodotto dal livello di presentazione.

Creare un nuovo file di classe denominato `ProductsBLLWithSprocs.cs` nel `~/App_Code/BLL` cartella e aggiungervi il codice seguente:


[!code-csharp[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample11.cs)]

Questa classe è in grado di simulare il `ProductsBLL` classe semantica da esercitazioni precedenti, ma utilizza il `ProductsTableAdapter` e `ProductsDataTable` oggetti dal `NorthwindWithSprocs` set di dati. Ad esempio, anziché con un `using NorthwindTableAdapters` istruzione all'inizio del file di classe come `ProductsBLL` avviene, il `ProductsBLLWithSprocs` utilizzato dalla classe `using NorthwindWithSprocsTableAdapters`. Analogamente, il `ProductsDataTable` e `ProductsRow` gli oggetti utilizzati in questa classe sono preceduti il `NorthwindWithSprocs` dello spazio dei nomi. Il `ProductsBLLWithSprocs` classe fornisce metodi di accesso ai dati di due `GetProducts` e `GetProductByProductID`, e metodi per l'aggiunta, aggiornamento ed eliminazione di un'istanza di un singolo prodotto.

## <a name="step-7-working-with-thenorthwindwithsprocsdataset-from-the-presentation-layer"></a>Passaggio 7: Utilizzo di`NorthwindWithSprocs`set di dati dal livello di presentazione

A questo punto è stata creata DAL che utilizza le stored procedure per accedere e modificare i dati del database sottostante. Abbiamo anche costruito un BLL rudimentali con metodi per recuperare tutti i prodotti o in un determinato prodotto insieme ai metodi per l'aggiunta, aggiornamento e l'eliminazione di prodotti. Per l'arrotondamento in questa esercitazione, s consentono di creare una pagina ASP.NET che utilizza il livello Business LOGIC `ProductsBLLWithSprocs` classe per la visualizzazione, aggiornamento ed eliminazione di record.

Aprire il `NewSprocs.aspx` nella pagina di `AdvancedDAL` cartelle e trascinare un controllo GridView dalla casella degli strumenti nella finestra di progettazione, denominarla `Products`. Smart tag di s GridView scegliere da associare a un nuovo oggetto ObjectDataSource denominato `ProductsDataSource`. Configurare ObjectDataSource per utilizzare il `ProductsBLLWithSprocs` classe, come illustrato nella figura 22.


[![Configurare ObjectDataSource per utilizzare la classe ProductsBLLWithSprocs](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image51.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image50.png)

**Figura 22**: configurare ObjectDataSource per utilizzare il `ProductsBLLWithSprocs` classe ([fare clic per visualizzare l'immagine ingrandita](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image52.png))


L'elenco a discesa nella scheda Seleziona è disponibili due opzioni, `GetProducts` e `GetProductByProductID`. Poiché si desidera visualizzare tutti i prodotti in GridView, scegliere il `GetProducts` metodo. Gli elenchi a discesa le schede UPDATE, INSERT e DELETE avere solo un metodo. Verificare che ognuno di questi elenchi a discesa dispone di un metodo appropriato selezionato e quindi fare clic su Fine.

Una volta completata la procedura guidata ObjectDataSource, Visual Studio aggiungerà BoundField e un CheckBoxField a GridView per i campi di dati di prodotto. Attivare la modifica di GridView s predefinite e l'eliminazione funzionalità selezionando le opzioni di Abilita eliminazione presente nello smart tag e abilitare la modifica.


[![La pagina contiene un controllo GridView con modifica e l'eliminazione di supporto abilitato](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image54.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image53.png)

**Nella figura 23**: la pagina contiene un controllo GridView con modifica e abilitato il supporto per l'eliminazione ([fare clic per visualizzare l'immagine ingrandita](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image55.png))


Come si ve descritti in esercitazioni precedenti, al termine della procedura guidata ObjectDataSource s, set di Visual Studio il `OldValuesParameterFormatString` proprietà originale\_{0}. Questa operazione deve essere ripristinato al valore predefinito di {0} affinché le funzionalità di modifica dei dati di lavoro assegnato correttamente i parametri previsti dai metodi nel nostro BLL. Pertanto, assicurarsi di impostare il `OldValuesParameterFormatString` proprietà {0} o rimuovere completamente la proprietà con la sintassi dichiarativa.

Dopo aver completato la configurazione guidata origine dati, attivare la modifica e l'eliminazione di supporto in GridView e la restituzione s ObjectDataSource `OldValuesParameterFormatString` proprietà sul valore predefinito, il markup dichiarativo pagina s dovrebbe essere simile al seguente:


[!code-aspx[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample12.aspx)]

A questo punto è stato possibile riordinarne GridView personalizzando l'interfaccia di modifica per includere la convalida, con la `CategoryID` e `SupplierID` colonne sottoposti a rendering come controlli DropDownList e così via. È anche possibile aggiungere un messaggio di conferma sul lato client per il pulsante Elimina e si consiglia di prendere il tempo necessario per implementare tali miglioramenti. Poiché questi argomenti sono state descritte nelle esercitazioni precedenti, tuttavia, verranno trattati li nuovamente qui.

Indipendentemente dal fatto che è migliorare GridView o non, testare le funzionalità di base di pagina s in un browser. Come illustrato nella figura 24, nella pagina vengono elencati i prodotti in un controllo GridView che fornisce per riga modifica ed eliminazione di funzionalità.


[![I prodotti possono essere visualizzati, modificare ed eliminati dal controllo GridView.](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image57.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image56.png)

**Figura 24**: il possono essere visualizzati i prodotti, modificata ed eliminato dal controllo GridView ([fare clic per visualizzare l'immagine ingrandita](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image58.png))


## <a name="summary"></a>Riepilogo

Gli oggetti TableAdapter in un DataSet tipizzato possono accedere ai dati, dal database tramite istruzioni SQL ad hoc o tramite le stored procedure. Quando si utilizzano stored procedure, una stored procedure esistente da utilizzare o è possibile indicare la configurazione guidata TableAdapter per creare una nuova stored procedure in base a un `SELECT` query. In questa esercitazione, è esplorare come per le stored procedure create automaticamente.

Mentre le stored procedure generata automaticamente consente di risparmiare tempo, esistono casi in cui la stored procedure creata dalla procedura guidata t allinearlo cosa è verrebbe creata nel nostro. Ad esempio, il `Products_Update` stored procedure che prevede sia `@Original_ProductID` e `@ProductID` anche se i parametri di input di `@Original_ProductID` parametro è superfluo.

In molti scenari, le stored procedure potrebbero già essere state create oppure potrebbe essere necessario crearle manualmente in modo da avere un maggiore livello di controllo sui comandi s stored procedure. In entrambi i casi, si desidera indicare il TableAdapter utilizzare stored procedure esistenti per i relativi metodi. Le informazioni a questo scopo nella prossima esercitazione.

Buona programmazione!

## <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Creazione e gestione di Stored procedure](https://msdn.microsoft.com/library/aa214299(SQL.80).aspx)
- [Il recupero di dati scalare da una Stored Procedure](http://aspnet.4guysfromrolla.com/articles/062905-1.aspx)
- [Stored Procedure nozioni di base SQL Server](http://www.awprofessional.com/articles/article.asp?p=25288&amp;rl=1)
- [Stored procedure: Una panoramica](http://www.sqlteam.com/item.asp?ItemID=563)
- [Scrittura di una Stored Procedure](http://www.4guysfromrolla.com/webtech/111499-1.shtml)

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore per questa esercitazione è stata Geisenow Hilton. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[avanti](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)
