---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
title: Creazione di nuove Stored procedure per i TableAdapter del set di dati tipizzato (VB) | Microsoft Docs
author: rick-anderson
description: Nelle esercitazioni precedenti abbiamo creato istruzioni SQL nel codice e passare le istruzioni per il database deve essere eseguito. Un approccio alternativo consiste nell'usare s...
ms.author: riande
ms.date: 07/18/2007
ms.assetid: a5a4a9ba-d18d-489a-a6b0-a3c26d6b0274
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
msc.type: authoredcontent
ms.openlocfilehash: bc640564cfb67f0c1512bc7f4fae9ea7e6bc981f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835818"
---
<a name="creating-new-stored-procedures-for-the-typed-datasets-tableadapters-vb"></a>Creazione di nuove Stored procedure per i TableAdapter del set di dati tipizzato (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare il codice](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_67_VB.zip) o [Scarica il PDF](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/datatutorial67vb1.pdf)

> Nelle esercitazioni precedenti abbiamo creato istruzioni SQL nel codice e passare le istruzioni per il database deve essere eseguito. Un approccio alternativo consiste nell'utilizzare le stored procedure, in cui le istruzioni SQL sono predefinite a livello di database. In questa esercitazione è come avere la configurazione guidata TableAdapter generate nuove stored procedure per noi.


## <a name="introduction"></a>Introduzione

Data Access Layer (DAL) per queste esercitazioni Usa i set di dati tipizzato. Come descritto nel [creazione di un livello di accesso ai dati](../introduction/creating-a-data-access-layer-vb.md) esercitazione, i set di dati tipizzato costituiti da oggetti DataTable fortemente tipizzato e gli oggetti TableAdapter. Oggetti DataTable rappresentano le entità logiche nel sistema durante l'interfaccia di classe TableAdapter con il database sottostante per eseguire le operazioni di accesso ai dati. Ciò include la compilazione di oggetti DataTable con i dati, l'esecuzione di query che restituiscono dati scalari, inserimento, aggiornamento e l'eliminazione di record dal database.

I comandi SQL eseguiti per gli oggetti TableAdapter possono essere entrambe le istruzioni SQL ad hoc, ad esempio `SELECT columnList FROM TableName`, o stored procedure. Gli oggetti TableAdapter nella nostra architettura utilizzare istruzioni SQL ad hoc. Molti sviluppatori e amministratori di database, tuttavia, preferiscono la stored procedure le istruzioni SQL ad hoc per motivi di sicurezza, gestibilità e aggiornabilità. Altri preferiscono ardently istruzioni SQL ad hoc per loro flessibilità. Nel mio lavoro preferire le stored procedure su istruzioni SQL ad hoc, ma ha scelto di usare le istruzioni SQL ad hoc per semplificare le esercitazioni precedenti.

Quando la definizione di un oggetto TableAdapter o aggiunta di nuovi metodi, la configurazione guidata TableAdapter s rende altrettanto facile creare nuove stored procedure o utilizzare le stored procedure esistente come avviene per l'uso di istruzioni SQL ad hoc. In questa esercitazione verrà esaminato come ottenere la configurazione guidata TableAdapter s generare automaticamente stored procedure. Nella prossima esercitazione si esaminerà come configurare i metodi di s TableAdapter per utilizzare le stored procedure esistente o creato manualmente.

> [!NOTE]
> Vedere il post di blog di Rob Howard [Don usare Stored procedure ancora t?](http://grokable.com/2003/11/dont-use-stored-procedures-yet-must-be-suffering-from-nihs-not-invented-here-syndrome/) e [Frans Bouma](https://weblogs.asp.net/fbouma/) s post di blog [Stored procedure sono danneggiati, M Kay?](https://weblogs.asp.net/fbouma/archive/2003/11/18/38178.aspx) per un vivace dibattito su vantaggi e gli svantaggi di stored procedure e SQL ad hoc.


## <a name="stored-procedure-basics"></a>Nozioni di base di stored Procedure

Le funzioni sono un costrutto comune a tutti i linguaggi di programmazione. Una funzione è una raccolta di istruzioni che vengono eseguiti quando viene chiamata la funzione. Le funzioni possono accettare parametri di input e, facoltativamente, possono restituire un valore. *[Le stored procedure](http://en.wikipedia.org/wiki/Stored_procedure)*  sono costrutti di database che presentano molte similitudini con le funzioni nei linguaggi di programmazione. Una stored procedure è costituita da un set di istruzioni T-SQL che vengono eseguite quando viene chiamata la stored procedure. Una stored procedure può accettare zero a molti parametri di input e può restituire valori scalari, i parametri di output o, in genere, set di risultati da `SELECT` le query.

> [!NOTE]
> Le stored procedure sono spesso detta sprocs o Service Pack.


Vengono create stored procedure utilizzando il [ `CREATE PROCEDURE` ](https://msdn.microsoft.com/library/aa258259(SQL.80).aspx) istruzione T-SQL. Ad esempio, lo script T-SQL seguente crea una stored procedure denominata `GetProductsByCategoryID` che accetta un singolo parametro denominato `@CategoryID` e restituisce il `ProductID`, `ProductName`, `UnitPrice`, e `Discontinued` campi di tali colonne di `Products` tabella che dispone di un oggetto corrispondente `CategoryID` valore:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample1.sql)]

Dopo aver creata questa stored procedure, può essere chiamato usando la sintassi seguente:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample2.sql)]

> [!NOTE]
> Nella prossima esercitazione verrà esaminato la creazione di stored procedure tramite l'IDE di Visual Studio. Per questa esercitazione, tuttavia, si userà per consentire la configurazione guidata TableAdapter genera automaticamente le stored procedure per noi.


Oltre a restituire semplicemente i dati, le stored procedure vengono spesso utilizzate per eseguire più comandi di database all'interno dell'ambito di una singola transazione. Una stored procedure denominata `DeleteCategory`, ad esempio, potrebbero essere necessari un `@CategoryID` parametro ed eseguire due `DELETE` istruzioni: prima di tutto, uno per eliminare i prodotti correlati e un altro eliminando la categoria specificata. Più istruzioni all'interno di una stored procedure vengono *non* automaticamente sottoposta a wrapping in una transazione. Altri comandi T-SQL dovranno essere emesso per assicurarsi che la stored procedure s che più comandi vengono trattati come operazione atomica. Si vedrà come eseguire il wrapping di comandi una stored procedure all'interno dell'ambito di una transazione nell'esercitazione successiva.

Quando si utilizzano le stored procedure all'interno di un'architettura, il livello di accesso ai dati s richiama una particolare stored procedure, anziché eseguire un'istruzione SQL ad hoc. Consente di centralizzare la posizione delle istruzioni SQL eseguite (on database) anziché averlo definito all'interno dell'architettura s dell'applicazione. Questa centralizzazione senza dubbio rende più semplice individuare, analizzare e ottimizzare le query e offre un quadro molto più chiaro per quanto riguarda come e dove il database è in uso.

Per altre informazioni su nozioni fondamentali sulla stored procedure, consultare le risorse nella sezione ulteriori letture alla fine di questa esercitazione.

## <a name="step-1-creating-the-advanced-data-access-layer-scenarios-web-pages"></a>Passaggio 1: Creare le pagine Web di scenari livello di accesso di dati avanzati

Prima di iniziare la discussione sulla creazione di un utilizzo di stored procedure DAL, consentire s prima di tutto si consiglia di creare le pagine ASP.NET nel progetto di sito Web che è necessario per questa e le esercitazioni più avanti. Iniziare aggiungendo una nuova cartella denominata `AdvancedDAL`. Successivamente, aggiungere le seguenti pagine ASP.NET per quella cartella, assicurandosi di associare ogni pagina con il `Site.master` pagina master:

- `Default.aspx`
- `NewSprocs.aspx`
- `ExistingSprocs.aspx`
- `JOINs.aspx`
- `AddingColumns.aspx`
- `ComputedColumns.aspx`
- `EncryptingConfigSections.aspx`
- `ManagedFunctionsAndSprocs.aspx`


![Aggiungere le pagine ASP.NET per le esercitazioni di scenari livello accesso dati avanzati](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image1.png)

**Figura 1**: aggiungere le pagine ASP.NET per le esercitazioni di scenari livello accesso dati avanzati


In altre cartelle, analogo a `Default.aspx` nella `AdvancedDAL` cartella elencherà le esercitazioni nella relativa sezione. Si tenga presente che il `SectionLevelTutorialListing.ascx` controllo utente fornisce questa funzionalità. Pertanto, aggiungere questo controllo utente da `Default.aspx` trascinandolo da Esplora soluzioni nella pagina di visualizzazione della struttura s.


[![Aggiungere il controllo utente sectionleveltutoriallisting. ascx a default. aspx](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image3.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image2.png)

**Figura 2**: aggiungere i `SectionLevelTutorialListing.ascx` controllo utente da `Default.aspx` ([fare clic per visualizzare l'immagine con dimensioni normali](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image4.png))


Infine, aggiungere queste pagine come voci per il `Web.sitemap` file. In particolare, aggiungere il markup seguente dopo l'utilizzo dei dati in batch `<siteMapNode>`:


[!code-xml[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample3.xml)]

Dopo aver aggiornato `Web.sitemap`, si consiglia di visualizzare il sito Web di esercitazioni tramite un browser. Il menu a sinistra ora include elementi per le esercitazioni di scenari DAL avanzate.


![Mappa del sito include ora voci per le esercitazioni di scenari avanzati DAL](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image5.png)

**Figura 3**: la mappa del sito include ora voci per le esercitazioni di scenari avanzati DAL


## <a name="step-2-configuring-a-tableadapter-to-create-new-stored-procedures"></a>Passaggio 2: Configurazione di un oggetto TableAdapter per creare nuove Stored procedure

Per illustrare la creazione di un livello di accesso ai dati che utilizza stored procedure anziché le istruzioni SQL ad hoc, s ti permettono di creare un nuovo DataSet tipizzati nelle `~/App_Code/DAL` cartella denominata `NorthwindWithSprocs.xsd`. Poiché è stato eseguito l'accesso durante il processo in dettaglio nelle esercitazioni precedenti, si procederà rapidamente tramite la procedura seguente. Rimanere bloccati o per richiedere un'ulteriore istruzioni di creazione e configurazione di un DataSet tipizzato, vedere la [creazione di un livello di accesso ai dati](../introduction/creating-a-data-access-layer-vb.md) esercitazione.

Aggiungere un nuovo set di dati al progetto facendo clic su di `DAL` cartella, scegliere Aggiungi nuovo elemento e selezionando il modello di set di dati, come illustrato nella figura 4.


[![Aggiungere un nuovo set di dati tipizzato al progetto denominato NorthwindWithSprocs.xsd](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image7.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image6.png)

**Figura 4**: aggiungere un nuovo set di dati tipizzato per il progetto denominato `NorthwindWithSprocs.xsd` ([fare clic per visualizzare l'immagine con dimensioni normali](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image8.png))


Questo verrà creare il nuovo set di dati tipizzato, aprire la finestra di progettazione, creare un nuovo TableAdapter e avviare la configurazione guidata TableAdapter. Il primo passaggio s di configurazione guidata TableAdapter chiede di selezionare il database da usare. La stringa di connessione al database Northwind dovrebbe essere elencata nell'elenco a discesa. Selezionare questa opzione e fare clic su Avanti.

In questa schermata successiva è possibile scegliere la modalità dell'oggetto TableAdapter di accesso del database. Nelle esercitazioni precedenti è selezionata la prima opzione, Usa istruzioni SQL. Per questa esercitazione, selezionare la seconda opzione, creare nuove stored procedure e fare clic su Avanti.


[![Indicare il TableAdpater per creare nuove Stored procedure](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image10.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image9.png)

**Figura 5**: indicare il TableAdpater per creare nuove Stored procedure ([fare clic per visualizzare l'immagine con dimensioni normali](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image11.png))


Analogamente all'utilizzo di istruzioni SQL ad hoc, a nel passaggio seguente ci viene richiesto di fornire il `SELECT` istruzione per la query principale s TableAdapter. Ma invece di usare la `SELECT` istruzione immessi qui per eseguire direttamente una query ad hoc, la configurazione guidata TableAdapter s creerà una stored procedure che contiene questo `SELECT` query.

Usare il comando seguente `SELECT` query per questo oggetto TableAdapter:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample4.sql)]


[![Immettere la Query SELECT](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image13.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image12.png)

**Figura 6**: immettere il `SELECT` Query ([fare clic per visualizzare l'immagine con dimensioni normali](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image14.png))


> [!NOTE]
> La query precedente si differenzia leggermente dalla query principale della `ProductsTableAdapter` nella `Northwind` DataSet tipizzato. Si tenga presente che il `ProductsTableAdapter` nella `Northwind` DataSet tipizzata include due sottoquery correlate per riportare online il nome di categoria e il nome della società per ogni categoria di prodotto s e fornitore. Nella imminente [aggiornamento del TableAdapter per usare join](updating-the-tableadapter-to-use-joins-vb.md) si esaminerà l'aggiunta di questa esercitazione i dati correlati a questo oggetto TableAdapter.


Si consiglia di fare clic sul pulsante Opzioni avanzate. Da qui è possibile specificare se la procedura guidata deve generare anche insert, update e istruzioni di eliminazione per i TableAdapter, se si desidera usare la concorrenza ottimistica e indica se la tabella di dati deve essere aggiornata dopo inserimenti e aggiornamenti. Per impostazione predefinita è selezionata l'opzione di istruzioni genera istruzioni Insert, Update e Delete. Lasciare selezionata. Per questa esercitazione, lasciare deselezionata Usa le opzioni di concorrenza ottimistica.

Quando si verificano le stored procedure create automaticamente la configurazione guidata TableAdapter, sembra che l'aggiornamento, l'opzione di tabella di dati viene ignorato. Indipendentemente dal fatto che questa casella di controllo è selezionata, il risultante insert e update stored procedure recuperano il record appena-inserite o aggiornate just, come vedremo nel passaggio 3.


![Lasciare le istruzioni genera istruzioni Insert, Update e Delete opzione selezionata](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image15.png)

**Figura 7**: lasciare le istruzioni genera istruzioni Insert, Update e Delete opzione selezionata


> [!NOTE]
> Se è selezionata l'opzione di usare la concorrenza ottimistica, la procedura guidata aggiungerà condizioni aggiuntive per il `WHERE` clausola che impedire l'aggiornamento se sono state eseguite modifiche in altri campi di dati. Fare riferimento al [implementazione della concorrenza ottimistica](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md) esercitazione per altre informazioni sull'uso della funzionalità di controllo della concorrenza ottimistica predefinita s TableAdapter.


Dopo aver immesso il `SELECT` eseguire una query e per confermare che sia selezionata l'opzione di istruzioni genera istruzioni Insert, Update e Delete, fare clic su Avanti. Questa schermata successiva, illustrata nella figura 8, richiesto per i nomi delle stored procedure che creerà la procedura guidata per la selezione, inserimento, aggiornamento ed eliminazione dei dati. Modifica i nomi di procedure per questi dati archiviati `Products_Select`, `Products_Insert`, `Products_Update`, e `Products_Delete`.


[![Rinominare le Stored procedure](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image17.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image16.png)

**Figura 8**: rinominare le Stored procedure ([fare clic per visualizzare l'immagine con dimensioni normali](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image18.png))


Per visualizzare la configurazione guidata TableAdapter verrà utilizzato per creare quattro stored procedure T-SQL, fare clic sul pulsante Anteprima Script SQL. Nella finestra di dialogo Anteprima Script SQL può salvare lo script in un file o copiarlo negli Appunti.


![Visualizzare in anteprima lo Script SQL usato per generare le Stored procedure](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image19.png)

**Figura 9**: anteprima Script SQL usato per generare le Stored procedure


Dopo la denominazione delle stored procedure, fare clic accanto ai metodi corrispondenti ai TableAdapter nome. Proprio come quando si Usa istruzioni SQL ad hoc, possiamo creare metodi che è riempire un DataTable esistenti o restituiscono una nuova. Possiamo inoltre specificare se l'oggetto TableAdapter deve includere il modello di DB-Direct per inserimento, aggiornamento ed eliminazione di record. Lasciare tutte le tre caselle di controllo selezionate, ma il valore restituito non rinominare un metodo di DataTable per `GetProducts` (come illustrato nella figura 10).


[![Denominare i metodi Fill e GetProducts](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image21.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image20.png)

**Figura 10**: nome dei metodi `Fill` e `GetProducts` ([fare clic per visualizzare l'immagine con dimensioni normali](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image22.png))


Fare clic su Avanti per visualizzare un riepilogo dei passaggi che eseguirà la procedura guidata. Completare la procedura guidata facendo clic sul pulsante Fine. Al termine della procedura guidata, verrà di nuovo per i DataSet Designer, che dovrebbe ora includere la `ProductsDataTable`.


[![Progettazione DataSet s Mostra il ProductsDataTable appena aggiunta](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image24.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image23.png)

**Figura 11**: s set di dati di progettazione Mostra le appena aggiunto `ProductsDataTable` ([fare clic per visualizzare l'immagine con dimensioni normali](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image25.png))


## <a name="step-3-examining-the-newly-created-stored-procedures"></a>Passaggio 3: Esaminare le Stored procedure appena create

La configurazione guidata TableAdapter usata nel passaggio 2 automaticamente create le stored procedure per la selezione, inserimento, aggiornamento ed eliminazione dei dati. Queste stored procedure possono essere visualizzate o modificate tramite Visual Studio da Esplora Server e il drill-down nella cartella database s Stored procedure. Come illustrato nella figura 12, il database di Northwind contiene quattro nuove stored procedure: `Products_Delete`, `Products_Insert`, `Products_Select`, e `Products_Update`.


![Le quattro Stored procedure create nel passaggio 2 sono reperibile nella cartella Database s Stored procedure](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image26.png)

**Figura 12**: quattro Stored procedure create nel passaggio 2 sono reperibile nella cartella Database s Stored procedure


> [!NOTE]
> Se non è possibile visualizzare Esplora Server, passare al menu Visualizza e scegliere l'opzione di Esplora Server. Se non è possibile visualizzare le relative al prodotto stored procedure aggiunte dal passaggio 2, provare a facendo clic sulla cartella Stored procedure e scegliendo di aggiornamento.


Per visualizzare o modificare una stored procedure, fare doppio clic sul relativo nome in Esplora Server o, in alternativa, fare doppio clic su nella stored procedure e scegliere Apri. Figura 13 Mostra il `Products_Delete` stored procedure, di apertura.


[![Le stored procedure possono essere aperto e modificate da Visual Studio](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image28.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image27.png)

**Figura 13**: Stored procedure possono essere aperti e modificati da all'interno di Visual Studio ([fare clic per visualizzare l'immagine con dimensioni normali](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image29.png))


Il contenuto di entrambe le `Products_Delete` e `Products_Select` stored procedure sono piuttosto semplici. Il `Products_Insert` e `Products_Update` stored procedure, d'altra parte, giustificano un esame più da vicino come entrambe eseguono un `SELECT` istruzione dopo il loro `INSERT` e `UPDATE` istruzioni. Ad esempio, il codice SQL seguente costituisce il `Products_Insert` stored procedure:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample5.sql)]

La stored procedure accetta come parametri di input il `Products` le colonne che sono state restituite dai `SELECT` query specificata nella procedura guidata s TableAdapter e questi valori vengono usati in un `INSERT` istruzione. Seguendo la `INSERT` istruzione, una `SELECT` query viene usata per restituire il `Products` i valori di colonna (incluso il `ProductID`) del record appena aggiunto. Questa funzionalità di aggiornamento è utile quando l'aggiunta di un nuovo record usando il modello di aggiornamento Batch che automaticamente Aggiorna appena aggiunta `ProductRow` istanze `ProductID` proprietà con i valori con incremento automatico assegnati dal database.

Il codice seguente illustra questa funzionalità. Contiene un `ProductsTableAdapter` e `ProductsDataTable` creata per il `NorthwindWithSprocs` DataSet tipizzato. Un nuovo prodotto viene aggiunto al database tramite la creazione di un `ProductsRow` istanza, specificando i relativi valori e chiamando i TableAdapter `Update` metodo, passando il `ProductsDataTable`. Internamente, i TableAdapter `Update` metodo enumera la `ProductsRow` istanze DataTable passato (in questo esempio è presente un solo - quello appena aggiunto) ed esegue appropriato di inserire, aggiornare o eliminare comandi. In questo caso, il `Products_Insert` stored procedure viene eseguita, che aggiunge un nuovo record per il `Products` di tabella e restituisce i dettagli del record appena aggiunto. Il `ProductsRow` istanza s `ProductID` valore viene quindi aggiornato. Dopo il `Update` completamento del metodo, è possibile accedere al record appena aggiunti s `ProductID` valore tramite il `ProductsRow` s `ProductID` proprietà.


[!code-vb[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample6.vb)]

Il `Products_Update` stored procedure in modo analogo include un `SELECT` istruzione dopo il relativo `UPDATE` istruzione.


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample7.sql)]

Si noti che questa stored procedure includono due parametri di input per `ProductID`: `@Original_ProductID` e `@ProductID`. Questa funzionalità permette scenari in cui la chiave primaria può essere modificata. Ad esempio, in un database di dipendenti, ogni record dipendente potrebbe usare il numero di previdenza sociale s dipendente come la propria chiave primaria. Per modificare un numero di previdenza sociale di s dipendente esistente, è necessario che venga forniti sia il nuovo numero di previdenza sociale e quella originale. Per il `Products` tabella, tale funzionalità non è necessaria perché la `ProductID` colonna è un `IDENTITY` colonna e non deve mai essere modificato. In effetti, il `UPDATE` istruzione il `Products_Update` includono stored procedure t il `ProductID` colonna nell'elenco di colonne. Pertanto, mentre `@Original_ProductID` viene utilizzata per il `UPDATE` istruzione s `WHERE` clausola è superfluo per il `Products` tabella e possono essere sostituiti dal `@ProductID` parametro. Quando si modifica un parametri delle stored procedure s è importante che vengono aggiornati anche i metodi TableAdapter che usano la stored procedure.

## <a name="step-4-modifying-a-stored-procedure-s-parameters-and-updating-the-tableadapter"></a>Passaggio 4: Modifica dei parametri s una Stored Procedure e aggiornamento del TableAdapter

Poiché il `@Original_ProductID` parametro è superflua, ti permettono di s rimuoverla dal `Products_Update` stored procedure di completamente. Aprire il `Products_Update` stored procedure elimina il `@Original_ProductID` parametro e, nel `WHERE` clausola del `UPDATE` istruzione, cambiare il nome del parametro utilizzato da `@Original_ProductID` a `@ProductID`. Dopo aver apportato queste modifiche, l'istruzione T-SQL della stored procedure dovrebbe essere simile al seguente:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample8.sql)]

Per salvare queste modifiche al database, fare clic sull'icona Salva nella barra degli strumenti o premere Ctrl + S. A questo punto, il `Products_Update` stored procedure non prevede un `@Original_ProductID` parametro di input, ma il TableAdapter sia configurato per passare un parametro di questo tipo. È possibile visualizzare i parametri dell'oggetto TableAdapter verrà inviato al `Products_Update` stored procedure di selezionando TableAdapter in Progettazione DataSet, passare alla finestra proprietà e facendo clic sui puntini di sospensione nel `UpdateCommand` s `Parameters` raccolta. Verrà visualizzata la finestra di dialogo Editor dell'insieme di parametri, illustrato nella figura 14.


![Gli elenchi di Editor della raccolta di parametri i parametri utilizzati passato al Products_Update Stored Procedure](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image30.png)

**Figura 14**: gli elenchi di Editor della raccolta di parametri i parametri utilizzati passato al `Products_Update` Stored Procedure


È possibile rimuovere questo parametro da qui, selezionando semplicemente il `@Original_ProductID` parametro dall'elenco dei membri e facendo clic sul pulsante Rimuovi.

In alternativa, è possibile aggiornare i parametri utilizzati per tutti i metodi, pulsante destro del mouse su TableAdapter nella finestra di progettazione e scegliere Configura. Verrà visualizzata la configurazione guidata TableAdapter, elencare le stored procedure utilizzate per la selezione, inserimento, aggiornamento, e l'eliminazione, insieme a parametri di stored procedure prevedono di ricevere. Se si fa clic nell'elenco a discesa di aggiornamenti è possibile vedere le `Products_Update` stored procedure prevede parametri di input, che ora non sono più inclusi `@Original_ProductID` (vedere Figura 15). È sufficiente fare clic su Fine per aggiornare automaticamente la raccolta di parametri utilizzata dall'oggetto TableAdapter.


[![In alternativa, è possibile utilizzare la configurazione guidata TableAdapter s per le raccolte di parametri di metodi di aggiornamento](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image32.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image31.png)

**Figura 15**: È possibile usare in alternativa ai TableAdapter configurazione guidata per aggiornare insiemi di parametri metodi relativo ([fare clic per visualizzare l'immagine con dimensioni normali](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image33.png))


## <a name="step-5-adding-additional-tableadapter-methods"></a>Passaggio 5: Aggiungere i metodi TableAdapter aggiuntive

Come passaggio 2 illustrato, quando si crea un nuovo TableAdapter è facile per la stored procedure corrispondenti vengono generati automaticamente. Lo stesso vale quando si aggiungono altri metodi a un oggetto TableAdapter. Per illustrare questo concetto, consentire s aggiungere una `GetProductByProductID(productID)` metodo di `ProductsTableAdapter` creato nel passaggio 2. Questo metodo utilizzerà come input un `ProductID` valore e restituire i dettagli relativi al prodotto specificato.

Avviare facendo clic sull'oggetto TableAdapter e scegliendo Aggiungi Query dal menu di scelta rapida.


![Aggiungere una nuova Query al TableAdapter.](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image34.png)

**Figura 16**: aggiungere una nuova Query al TableAdapter.


Verrà avviata la configurazione guidata Query TableAdapter, che richiede prima di tutto come oggetto TableAdapter deve accedere al database. Per una nuova stored procedure creata, scegliere Crea una nuova opzione della stored procedure e fare clic su Avanti.


[![Scegliere di creare una nuova stored procedure, opzione](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image36.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image35.png)

**Figura 17**: scegliere di creare una nuova stored procedure opzione ([fare clic per visualizzare l'immagine con dimensioni normali](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image37.png))


Nella schermata successiva viene chiesto di specificare per identificare il tipo di query da eseguire, se viene restituito un set di righe o un singolo valore scalare o eseguire un' `UPDATE`, `INSERT`, o `DELETE` istruzione. Poiché il `GetProductByProductID(productID)` metodo verrà restituita una riga, lasciare il SELECT che restituisce righe opzione selezionata e scegliere "Avanti".


[![Scegliere il SELECT che restituisce righe opzione](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image39.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image38.png)

**Figura 18**: Seleziona che restituisce righe opzione ([fare clic per visualizzare l'immagine con dimensioni normali](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image40.png))


Nella schermata successiva consente di visualizzare la query principale di s TableAdapter, che viene elencato solo il nome della stored procedure (`dbo.Products_Select`). Sostituire il nome della stored procedure con il codice seguente `SELECT` istruzione che restituisce tutti i campi di prodotto per un prodotto specificato:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample9.sql)]


[![Sostituire il nome della Stored Procedure con una Query SELECT](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image42.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image41.png)

**Figura 19**: sostituire il nome di Stored Procedure con un `SELECT` Query ([fare clic per visualizzare l'immagine con dimensioni normali](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image43.png))


La schermata successiva viene chiesto di assegnare un nome di stored procedure che verrà creata. Immettere il nome `Products_SelectByProductID` e fare clic su Avanti.


[![Denominare il nuovo Products_SelectByProductID Stored Procedure](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image45.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image44.png)

**Figura 20**: nome della nuova Stored Procedure `Products_SelectByProductID` ([fare clic per visualizzare l'immagine con dimensioni normali](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image46.png))


Il passaggio finale della procedura guidata consente di modificare il metodo nomi generati, nonché indicano se si desidera utilizzare il riempimento di un modello di DataTable, restituire una serie di DataTable, o entrambi. Per questo metodo, non modificare entrambe le opzioni selezionate, ma rinominare i metodi `FillByProductID` e `GetProductByProductID`. Fare clic su Avanti per visualizzare un riepilogo dei passaggi di eseguire la procedura guidata e quindi fare clic su Fine per completare la procedura guidata.


[![Rinominare i metodi di s TableAdapter FillByProductID e GetProductByProductID](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image48.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image47.png)

**Figura 21**: rinominare i metodi TableAdapter s `FillByProductID` e `GetProductByProductID` ([fare clic per visualizzare l'immagine con dimensioni normali](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image49.png))


Dopo aver completato la procedura guidata, TableAdapter dispone di un metodo di nuovo disponibile, `GetProductByProductID(productID)` che, quando richiamata, eseguirà il `Products_SelectByProductID` stored procedure che è stato appena creata. Richiedere qualche istante per visualizzare questa nuova stored procedure da Esplora Server drill-down della cartella Stored Procedures e aprendo `Products_SelectByProductID` (se non è visualizzato, fare doppio clic sulla cartella Stored procedure e scegliere Aggiorna).

Si noti che il `SelectByProductID` stored procedure accetta `@ProductID` come parametro di input ed esegue il `SELECT` istruzione che viene immesso nella procedura guidata.


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample10.sql)]

## <a name="step-6-creating-a-business-logic-layer-class"></a>Passaggio 6: Creazione di una classe di livello per la logica di Business

In tutta la serie di esercitazioni sono state strived mantenere un'architettura a livelli in cui il livello di presentazione apportate tutte le chiamate a livello BLL (Business LOGIC Layer). Per essere conformi a questa decisione progettuale, è necessario innanzitutto creare una classe BLL per il nuovo set di dati tipizzato prima che sia possibile accedere a dati del prodotto dal livello di presentazione.

Creare un nuovo file di classe denominato `ProductsBLLWithSprocs.vb` nella `~/App_Code/BLL` cartella e aggiungervi il codice seguente:


[!code-vb[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample11.vb)]

Questa classe è in grado di simulare il `ProductsBLL` classe semantica dalle esercitazioni precedenti, ma utilizza il `ProductsTableAdapter` e `ProductsDataTable` oggetti dal `NorthwindWithSprocs` set di dati. Ad esempio, anziché un `Imports NorthwindTableAdapters` istruzione all'inizio del file di classe come `ProductsBLL` affermativo, il `ProductsBLLWithSprocs` utilizzato dalla classe `Imports NorthwindWithSprocsTableAdapters`. Analogamente, il `ProductsDataTable` e `ProductsRow` gli oggetti utilizzati in questa classe sono preceduti il `NorthwindWithSprocs` dello spazio dei nomi. Il `ProductsBLLWithSprocs` classe fornisce due metodi, DAS `GetProducts` e `GetProductByProductID`, e i metodi per aggiungere, aggiornare ed eliminare un'istanza singola del prodotto.

## <a name="step-7-working-with-thenorthwindwithsprocsdataset-from-the-presentation-layer"></a>Passaggio 7: Uso del`NorthwindWithSprocs`set di dati dal livello di presentazione

A questo punto è stata creata DAL che utilizza stored procedure per accedere e modificare i dati del database sottostante. È stato creato anche un rudimentale BLL con i metodi per recuperare tutti i prodotti o un determinato prodotto insieme ai metodi per l'aggiunta, aggiornamento e l'eliminazione di prodotti. Per arrotondare disattivata in questa esercitazione, s ti permettono di creare una pagina ASP.NET che usa la s BLL `ProductsBLLWithSprocs` classe per la visualizzazione, aggiornamento ed eliminazione di record.

Aprire il `NewSprocs.aspx` nella pagina la `AdvancedDAL` cartelle e trascinare un controllo GridView dalla casella degli strumenti nella finestra di progettazione, denominarlo `Products`. Il controllo GridView tag smart s scegliere da associare a un nuovo oggetto ObjectDataSource denominato `ProductsDataSource`. Configurare ObjectDataSource per usare il `ProductsBLLWithSprocs` classe, come illustrato nella figura 22.


[![Configurare ObjectDataSource per usare la classe ProductsBLLWithSprocs](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image51.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image50.png)

**Figura 22**: configurare ObjectDataSource per usare il `ProductsBLLWithSprocs` classe ([fare clic per visualizzare l'immagine con dimensioni normali](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image52.png))


L'elenco di riepilogo a discesa della scheda selezionare ha due opzioni `GetProducts` e `GetProductByProductID`. Poiché si desidera visualizzare tutti i prodotti in GridView, scegliere il `GetProducts` (metodo). Gli elenchi a discesa nelle schede UPDATE, INSERT e DELETE presente un solo metodo. Assicurarsi che ciascuno di questi elenchi a discesa dispone di un metodo appropriato selezionato e quindi fare clic su Fine.

Dopo aver completato la procedura guidata ObjectDataSource, Visual Studio aggiungerà BoundField e un CampoCasellaDiControllo a GridView per i campi di dati del prodotto. Attivare le funzionalità di eliminazione e GridView s incorporati di modifica selezionando le Abilita modifica e le opzioni Abilita eliminazione presente nello smart tag.


[![La pagina contiene un controllo GridView con modifica e l'eliminazione di supporto abilitato](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image54.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image53.png)

**Figura 23**: la pagina contiene un controllo GridView con modifica e l'eliminazione di supporto abilitato ([fare clic per visualizzare l'immagine con dimensioni normali](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image55.png))


Come abbiamo ve illustrati nelle esercitazioni precedenti, al termine della procedura guidata ObjectDataSource s, set di Visual Studio il `OldValuesParameterFormatString` proprietà originale\_{0}. Questa operazione deve essere ripristinato al valore predefinito di {0} affinché le funzionalità di modifica dei dati per il corretto funzionamento specificato i parametri previsti dai metodi nel nostro BLL. Pertanto, assicurarsi di impostare il `OldValuesParameterFormatString` proprietà {0} oppure rimuovere completamente la proprietà con la sintassi dichiarativa.

Dopo aver completato la procedura guidata Configura origine dati, attivare la modifica e l'eliminazione di supporto in GridView e restituendo la s ObjectDataSource `OldValuesParameterFormatString` proprietà sul valore predefinito, il markup dichiarativo pagina s dovrebbe essere simile al seguente:


[!code-aspx[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample12.aspx)]

A questo punto è stato possibile riordinarne GridView personalizzando l'interfaccia di modifica per includere la convalida, con la `CategoryID` e `SupplierID` sottoposti a rendering come controlli DropDownList, colonne e così via. È anche possibile aggiungere una conferma lato client per il pulsante di eliminazione e ti invitiamo al tempo necessario per implementare tali miglioramenti. Poiché questi argomenti sono stati trattati nelle esercitazioni precedenti, tuttavia, non copriremo li nuovo qui.

Indipendentemente dal fatto che è ottimizzato GridView o non, testare le funzionalità di base di pagina s in un browser. Come illustrato nella figura 24, nella pagina vengono elencati i prodotti in un controllo GridView che fornisce ogni riga di modifica ed eliminazione di funzionalità.


[![I prodotti possono essere visualizzati, modificati ed eliminati dal controllo GridView.](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image57.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image56.png)

**Figura 24**: The possono essere visualizzati i prodotti, modificata e Deleted da GridView ([fare clic per visualizzare l'immagine con dimensioni normali](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image58.png))


## <a name="summary"></a>Riepilogo

Gli oggetti TableAdapter in un set di dati tipizzati possono accedere ai dati dal database tramite istruzioni SQL ad hoc o tramite le stored procedure. Quando si utilizzano le stored procedure, una stored procedure esistenti possono essere utilizzate o è possibile indicare la configurazione guidata TableAdapter per creare nuove stored procedure basate su un `SELECT` query. In questa esercitazione è stato illustrato come le stored procedure vengono creati automaticamente per noi.

Mantenendo le stored procedure generata automaticamente consente di risparmiare tempo, esistono casi in cui la stored procedure creata dalla procedura guidata t allineare con ciò che si sarebbero creati da soli. Un esempio è il `Products_Update` stored procedure, che sia previsto `@Original_ProductID` e `@ProductID` anche se i parametri di input di `@Original_ProductID` parametro è superfluo.

In molti scenari, le stored procedure sia già state create oppure potrebbe essere necessario compilarle manualmente in modo da avere un miglior grado di controllare i comandi s stored procedure. In entrambi i casi, si potrebbe richiedere del TableAdapter utilizzare stored procedure esistenti per i relativi metodi. Vogliamo vediamo come eseguire questa operazione nella prossima esercitazione.

Buona programmazione!

## <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Creazione e gestione le Stored procedure](https://msdn.microsoft.com/library/aa214299(SQL.80).aspx)
- [Il recupero di dati scalare da una Stored Procedure](http://aspnet.4guysfromrolla.com/articles/062905-1.aspx)
- [Stored Procedure nozioni di base di SQL Server](http://www.awprofessional.com/articles/article.asp?p=25288&amp;rl=1)
- [Stored procedure: Una panoramica](http://www.sqlteam.com/item.asp?ItemID=563)
- [La scrittura di una Stored Procedure](http://www.4guysfromrolla.com/webtech/111499-1.shtml)

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore capo di questa esercitazione è stata Hilton Geisenow. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs.md)
> [Successivo](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
