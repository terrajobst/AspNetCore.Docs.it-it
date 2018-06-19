---
uid: web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-vb
title: Aggiunta e la risposta ai pulsanti per un controllo GridView (VB) | Documenti Microsoft
author: rick-anderson
description: In questa esercitazione viene illustrato come aggiungere pulsanti personalizzati, a un modello e ai campi di un controllo GridView o DetailsView. In particolare, ti invieremo bui...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: 06c6bbd2-4bdc-435b-87a3-df2c868f4baa
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-vb
msc.type: authoredcontent
ms.openlocfilehash: 58b570c897810eeaa182a201616a182c02e9d92c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878100"
---
<a name="adding-and-responding-to-buttons-to-a-gridview-vb"></a>Aggiunta e la risposta ai pulsanti per un controllo GridView (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare App di esempio](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_28_VB.exe) o [Scarica il PDF](adding-and-responding-to-buttons-to-a-gridview-vb/_static/datatutorial28vb1.pdf)

> In questa esercitazione viene illustrato come aggiungere pulsanti personalizzati, a un modello e ai campi di un controllo GridView o DetailsView. In particolare, verrà creata un'interfaccia con un controllo FormView che consente all'utente di spostarsi tra i fornitori.


## <a name="introduction"></a>Introduzione

Sebbene molti scenari di report comportano l'accesso di sola lettura ai dati del report, è s non è insolito per i report includere la possibilità di eseguire azioni in base ai dati visualizzati. In genere ciò ha richiesto l'aggiunta di un controllo pulsante, LinkButton o ImageButton Web con ciascun record visualizzato nel report che, quando si fa clic, provoca un postback e richiama codice lato server. Modifica ed eliminazione di dati in base a un record è l'esempio più comune. In realtà, come abbiamo visto a partire dal [Panoramica di inserimento, aggiornamento ed eliminazione di dati](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) esercitazione, modifica ed eliminazione è infrequente che i controlli di GridView, DetailsView e FormView possono supportare tale funzionalità senza il necessario per la scrittura di una singola riga di codice.

Per modificare ed eliminare i pulsanti, GridView, DetailsView e FormView inoltre i controlli possono includere anche i pulsanti, LinkButton o ImageButtons che, quando si fa clic, eseguire una logica sul lato server personalizzata. In questa esercitazione viene illustrato come aggiungere pulsanti personalizzati, a un modello e ai campi di un controllo GridView o DetailsView. In particolare, verrà creata un'interfaccia con un controllo FormView che consente all'utente di spostarsi tra i fornitori. Per un dato fornitore FormView verranno visualizzate informazioni relative al fornitore insieme a un controllo pulsante Web che, se selezionato, verrà contrassegnati tutti i relativi prodotti associati come non più disponibile. Inoltre, un controllo GridView sono elencati i prodotti specificati dal fornitore selezionato, in cui ogni riga che contiene il prezzo di aumentare e pulsanti prezzo scontato che, se si fa clic, aumentare o ridurre il prodotto s `UnitPrice` del 10% (vedere la figura 1).


[![FormView e GridView contengono pulsanti che eseguono azioni personalizzate](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image2.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image1.png)

**Figura 1**: entrambe FormView e GridView contengono pulsanti che eseguire azioni personalizzate ([fare clic per visualizzare l'immagine ingrandita](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image3.png))


## <a name="step-1-adding-the-button-tutorial-web-pages"></a>Passaggio 1: Aggiunta di pagine Web esercitazione pulsante

Prima di esaminare come aggiungere i pulsanti personalizzati, consentire s attentamente prima di creare le pagine ASP.NET nel progetto sito Web che è necessario per questa esercitazione. Per iniziare, aggiungere una nuova cartella denominata `CustomButtons`. Successivamente, aggiungere le seguenti due pagine ASP.NET a quella cartella, assicurandosi di associare ogni pagina con il `Site.master` pagina master:

- `Default.aspx`
- `CustomButtons.aspx`


![Aggiungere le pagine ASP.NET per le esercitazioni relative pulsanti personalizzate](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image4.png)

**Figura 2**: aggiungere le pagine ASP.NET per le esercitazioni relative pulsanti personalizzate


Come in altre cartelle, `Default.aspx` nel `CustomButtons` cartella elencherà le esercitazioni nella relativa sezione. Tenere presente che il `SectionLevelTutorialListing.ascx` controllo utente fornisce questa funzionalità. Pertanto, aggiungere il controllo utente `Default.aspx` trascinandolo da Esplora soluzioni nella pagina di visualizzazione della struttura s.


[![Aggiungere il controllo utente SectionLevelTutorialListing.ascx Default.aspx](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image6.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image5.png)

**Figura 3**: aggiungere il `SectionLevelTutorialListing.ascx` controllo utente al `Default.aspx` ([fare clic per visualizzare l'immagine ingrandita](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image7.png))


Infine, aggiungere le pagine come voci per il `Web.sitemap` file. In particolare, aggiungere il markup seguente dopo il Paging e l'ordinamento `<siteMapNode>`:


[!code-xml[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample1.xml)]

Dopo aver aggiornato `Web.sitemap`, dedicare alcuni minuti per visualizzare il sito Web esercitazioni tramite un browser. Il menu a sinistra include elementi per la modifica, inserimento ed eliminazione di esercitazioni.


![Mappa del sito include ora la voce per l'esercitazione di pulsanti personalizzati](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image8.png)

**Figura 4**: mappa del sito include ora la voce per l'esercitazione di pulsanti personalizzati


## <a name="step-2-adding-a-formview-that-lists-the-suppliers"></a>Passaggio 2: Aggiunta di un controllo FormView che elenca i fornitori

Consente di iniziare questa esercitazione aggiungendo FormView che elenca i fornitori s. Come descritto nell'introduzione, questo controllo FormView consentirà all'utente di spostarsi tra i fornitori, che mostra i prodotti specificati dal fornitore in un controllo GridView. Inoltre, questo controllo FormView includerà un pulsante che, quando si fa clic, viene contrassegnato tutti i prodotti fornitore s come non più disponibile. Prima abbiamo riguardano effettuata con l'aggiunta di pulsante personalizzato al controllo FormView, consentire s prima di tutto creare solo FormView in modo che vengano visualizzate le informazioni del fornitore.

Aprire il `CustomButtons.aspx` nella pagina di `CustomButtons` cartella. Aggiungere un controllo FormView trascinandolo dalla casella degli strumenti di progettazione e il set relativo `ID` proprietà `Suppliers`. Dal FormView s smart tag, scegliere di creare un nuovo oggetto ObjectDataSource denominato `SuppliersDataSource`.


[![Creare un nuovo oggetto ObjectDataSource denominato SuppliersDataSource](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image10.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image9.png)

**Figura 5**: creare un nuovo denominato ObjectDataSource `SuppliersDataSource` ([fare clic per visualizzare l'immagine ingrandita](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image11.png))


Configurare il nuovo oggetto ObjectDataSource in modo che viene eseguita una query dal `SuppliersBLL` classe s `GetSuppliers()` (metodo) (vedere Figura 6). Poiché questo FormView non fornisce un'interfaccia per l'aggiornamento delle informazioni fornitore selezionare opzione (nessuno) nell'elenco di riepilogo a discesa nella scheda aggiornamenti.


[![Configurare l'origine dati per utilizzare la classe SuppliersBLL s GetSuppliers() (metodo)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image13.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image12.png)

**Figura 6**: configurare l'origine dati da utilizzare il `SuppliersBLL` classe s `GetSuppliers()` metodo ([fare clic per visualizzare l'immagine ingrandita](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image14.png))


Dopo aver configurato il ObjectDataSource, Visual Studio genererà un `InsertItemTemplate`, `EditItemTemplate`, e `ItemTemplate` per FormView. Rimuovere il `InsertItemTemplate` e `EditItemTemplate` e modificare il `ItemTemplate` in modo che venga visualizzato solo il fornitore s società nome e numero telefonico. Infine, attivare il supporto di paging per FormView selezionando la casella di controllo Abilita Paging smart tag (o impostando il relativo `AllowPaging` proprietà `True`). Dopo aver apportato queste modifiche markup dichiarativo s pagina dovrebbe essere simile al seguente:


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample2.aspx)]

Figura 7 illustra la pagina CustomButtons.aspx quando viene visualizzato tramite un browser.


[![FormView sono elencati i CompanyName e campi di telefono dal fornitore attualmente selezionato](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image16.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image15.png)

**Figura 7**: il controllo FormView Elenca il `CompanyName` e `Phone` i campi dal fornitore attualmente selezionato ([fare clic per visualizzare l'immagine ingrandita](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image17.png))


## <a name="step-3-adding-a-gridview-that-lists-the-selected-supplier-s-products"></a>Passaggio 3: Aggiunta di un controllo GridView in cui sono elencati i prodotti s fornitore selezionato

Prima di interrompere tutti i prodotti pulsante è aggiungere al modello s FormView, consentire s prima di aggiungere un controllo GridView sotto il controllo FormView che elenca i prodotti specificati dal fornitore selezionato. A tale scopo, aggiungere un controllo GridView alla pagina, impostare il relativo `ID` proprietà `SuppliersProducts`, e aggiungere un nuovo oggetto ObjectDataSource denominato `SuppliersProductsDataSource`.


[![Creare un nuovo oggetto ObjectDataSource denominato SuppliersProductsDataSource](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image19.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image18.png)

**Figura 8**: creare un nuovo denominato ObjectDataSource `SuppliersProductsDataSource` ([fare clic per visualizzare l'immagine ingrandita](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image20.png))


Configurare ObjectDataSource per utilizzare la classe ProductsBLL s `GetProductsBySupplierID(supplierID)` (metodo) (vedere Figura 9). Durante questo GridView consentirà un prezzo del prodotto s regolare, in quanto ha t utilizzando incorporati, modifica o eliminazione di funzionalità dal controllo GridView. Pertanto, è possibile impostare l'elenco a discesa su (nessuno) per s ObjectDataSource schede UPDATE, INSERT e DELETE.


[![Configurare l'origine dati per utilizzare la classe ProductsBLL s GetProductsBySupplierID(supplierID) (metodo)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image22.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image21.png)

**Figura 9**: configurare l'origine dati da utilizzare il `ProductsBLL` classe s `GetProductsBySupplierID(supplierID)` metodo ([fare clic per visualizzare l'immagine ingrandita](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image23.png))


Poiché il `GetProductsBySupplierID(supplierID)` metodo accetta un parametro di input, la procedura guidata ObjectDataSource richiede Microsoft per l'origine del valore di questo parametro. Per passare il `SupplierID` valore FormView, impostare l'elenco di riepilogo a discesa Origine parametro di controllo e l'elenco a discesa ControlID `Suppliers` (l'ID del controllo FormView creato nel passaggio 2).


[![Indicare che il supplierID parametro deve provenire dal controllo FormView Suppliers](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image25.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image24.png)

**Figura 10**: indicano che il *`supplierID`* parametro deve provenire dal `Suppliers` controllo FormView ([fare clic per visualizzare l'immagine ingrandita](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image26.png))


Dopo aver completato la procedura guidata ObjectDataSource, GridView conterrà un BoundField o CheckBoxField per ogni prodotto s campi di dati. Consentono di s trim questo verso il basso per visualizzare solo il `ProductName` e `UnitPrice` BoundField insieme al `Discontinued` CheckBoxField; consente, inoltre, il formato s il `UnitPrice` BoundField in modo che il testo sia formattato come valuta. Il GridView e `SuppliersProductsDataSource` markup dichiarativo di ObjectDataSource s dovrebbe essere simile al markup seguente:


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample3.aspx)]

A questo punto l'esercitazione consente di visualizzare un report master/dettagli, che consente all'utente di selezionare un fornitore dal FormView all'inizio e di visualizzare i prodotti forniti da tale fornitore tramite GridView nella parte inferiore. Figura 11 mostra una cattura di schermata della pagina quando si seleziona il fornitore Tokyo Traders FormView.


[![I prodotti s fornitore selezionato vengono visualizzati nel controllo GridView.](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image28.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image27.png)

**Figura 11**: i prodotti s il fornitore selezionato vengono visualizzati in GridView ([fare clic per visualizzare l'immagine ingrandita](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image29.png))


## <a name="step-4-creating-dal-and-bll-methods-to-discontinue-all-products-for-a-supplier"></a>Passaggio 4: Creazione DAL e BLL metodi per sospendere tutti i prodotti per un fornitore

Per poter aggiungere un pulsante in FormView che, quando si fa clic, interrompe tutti i prodotti fornitore s, è innanzitutto necessario aggiungere un metodo sia DAL e BLL che esegue questa azione. In particolare, questo metodo verrà denominato `DiscontinueAllProductsForSupplier(supplierID)`. Quando si fa clic sul pulsante s FormView, si verrà richiamare questo metodo nel livello di logica di Business, il passaggio di fornitore selezionato s `SupplierID`; il livello Business LOGIC chiamerà quindi verso il basso per il corrispondente metodo livello di accesso ai dati, verrà generato un `UPDATE` dell'istruzione il database che interrompe i prodotti del fornitore specificato s.

Come in corso delle esercitazioni precedenti, si userà un approccio dal basso in alto, a partire da creare il metodo DAL, quindi il metodo BLL e infine l'implementazione della funzionalità in una pagina ASP.NET. Aprire il `Northwind.xsd` DataSet tipizzato nel `App_Code/DAL` cartella e aggiungere un nuovo metodo per il `ProductsTableAdapter` (fare clic su di `ProductsTableAdapter` e scegliere Aggiungi Query). In questo modo verrà visualizzata la configurazione guidata Query TableAdapter, che ci si illustra il processo di aggiunta del nuovo metodo. Per iniziare, che indica che il metodo DAL utilizzerà un'istruzione SQL ad hoc.


[![Creare il metodo DAL utilizzando un'istruzione SQL Ad Hoc](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image31.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image30.png)

**Figura 12**: creare il metodo DAL usando un'istruzione SQL Ad Hoc ([fare clic per visualizzare l'immagine ingrandita](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image32.png))


Successivamente, verrà richiesto di Microsoft per il tipo di query da creare. Poiché il `DiscontinueAllProductsForSupplier(supplierID)` metodo sarà necessario aggiornare il `Products` tabella di database, l'impostazione di `Discontinued` campo su 1 per tutti i prodotti forniti dall'oggetto *`supplierID`*, è necessario creare una query che aggiorna i dati.


[![Scegliere il tipo di Query di aggiornamento](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image34.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image33.png)

**Figura 13**: scegliere il tipo di Query di aggiornamento ([fare clic per visualizzare l'immagine ingrandita](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image35.png))


Schermata di procedura guidata fornisce s TableAdapter esistente `UPDATE` istruzione, che aggiorna tutti i campi definiti nel `Products` DataTable. Sostituire il testo della query con l'istruzione seguente:


[!code-sql[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample4.sql)]

Dopo l'immissione di questa query e fare clic su Avanti, l'ultima schermata della procedura guidata viene richiesto per il nuovo nome di metodo s utilizzare `DiscontinueAllProductsForSupplier`. Completare la procedura guidata facendo clic sul pulsante Fine. All'uscita alla finestra di Progettazione DataSet verrà visualizzato un nuovo metodo nella `ProductsTableAdapter` denominato `DiscontinueAllProductsForSupplier(@SupplierID)`.


[![Denominare il nuovo DiscontinueAllProductsForSupplier DAL metodo](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image37.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image36.png)

**Figura 14**: nome del nuovo metodo DAL `DiscontinueAllProductsForSupplier` ([fare clic per visualizzare l'immagine ingrandita](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image38.png))


Con il `DiscontinueAllProductsForSupplier(supplierID)` metodo creato nel livello di accesso ai dati, l'attività successiva consiste nel creare il `DiscontinueAllProductsForSupplier(supplierID)` metodo nel livello di logica di Business. A tale scopo, aprire il `ProductsBLL` file di classe e aggiungere le seguenti:


[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample5.vb)]

Questo metodo chiama semplicemente verso il basso per il `DiscontinueAllProductsForSupplier(supplierID)` metodo di DAL, passando l'oggetto fornito *`supplierID`* valore del parametro. Se vi sono tutte le regole di business che è consentito solo un fornitore prodotti non viene più utilizzata in determinate circostanze, tali regole devono essere implementate in questo caso, il livello Business LOGIC.

> [!NOTE]
> A differenza di `UpdateProduct` overload nel `ProductsBLL` (classe), il `DiscontinueAllProductsForSupplier(supplierID)` firma del metodo non include il `DataObjectMethodAttribute` attributo (`<System.ComponentModel.DataObjectMethodAttribute(System.ComponentModel.DataObjectMethodType.Update, Boolean)>`). In questo modo il `DiscontinueAllProductsForSupplier(supplierID)` metodo dall'elenco a discesa ObjectDataSource s s procedura guidata Configura origine dati nella scheda aggiornamenti. Si va omesso l'attributo perché verrà chiamato il `DiscontinueAllProductsForSupplier(supplierID)` metodo direttamente da un gestore eventi nella nostra pagina ASP.NET.


## <a name="step-5-adding-a-discontinue-all-products-button-to-the-formview"></a>Passaggio 5: Aggiunta di un interrompere tutti i prodotti pulsante FormView

Con il `DiscontinueAllProductsForSupplier(supplierID)` completare BLL e DAL metodo, il passaggio finale per l'aggiunta della possibilità di sospendere tutti i prodotti per il fornitore selezionato consiste nell'aggiungere un controllo pulsante Web al controllo FormView s `ItemTemplate`. S consentono di aggiungere questi pulsanti sotto il numero di telefono fornitore s con il testo del pulsante, interrompere tutti i prodotti e un `ID` valore della proprietà `DiscontinueAllProductsForSupplier`. È possibile aggiungere questo controllo Web tramite la finestra di progettazione facendo clic sul collegamento Modifica modelli nello smart tag FormView s (vedere Figura 15), o direttamente tramite la sintassi dichiarativa.


[![Aggiungere un interrompere tutti i prodotti pulsante Web controllo FormView s ItemTemplate](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image40.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image39.png)

**Figura 15**: aggiungere interrompere tutti i prodotti Web un pulsante con la s FormView `ItemTemplate` ([fare clic per visualizzare l'immagine ingrandita](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image41.png))


Quando viene scelto il pulsante da visitare un utente viene utilizzata la pagina, un postback e s FormView [ `ItemCommand` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formview.itemcommand.aspx) generato. Per eseguire codice personalizzato in risposta a questo pulsante è disabilitato, è possibile creare un gestore eventi per questo evento. Tenere presente, tuttavia, che il `ItemCommand` evento viene generato ogni volta che *qualsiasi* si fa clic sul controllo Button, LinkButton o ImageButton Web in FormView. Ciò significa che quando l'utente da una pagina a altra in FormView, il `ItemCommand` viene generato l'evento; stessa operazione quando l'utente fa clic su Nuovo, modificare o eliminare in un controllo FormView che supporta l'inserimento, aggiornamento o eliminazione.

Poiché il `ItemCommand` generato indipendentemente dal fatto si fa clic sul pulsante, nel caso in cui gestore è necessario un modo per determinare se interrompere tutti i prodotti è stato fatto clic sul pulsante o se si trattasse di un altro pulsante. A tale scopo, è possibile impostare il controllo pulsante Web s `CommandName` proprietà su un valore di identificazione. Quando viene scelto il pulsante, questo `CommandName` valore viene passato il `ItemCommand` gestore dell'evento, consentono di determinare se interrompere tutti i prodotti pulsante è stato fatto clic sul pulsante. Impostare il s interrompere tutti i prodotti pulsante `CommandName` proprietà DiscontinueProducts.

Infine, consentire s utilizzare una finestra di dialogo di conferma sul lato client per assicurarsi che l'utente vuole interrompere i prodotti selezionati fornitore s. Come illustrato nel [aggiunta sul lato Client conferma quando l'eliminazione di](../editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-vb.md) dell'esercitazione, ciò può essere eseguita con un bit di JavaScript. In particolare, impostare la proprietà di OnClientClick s controllo pulsante Web su `return confirm('This will mark _all_ of this supplier\'s products as discontinued. Are you certain you want to do this?');`

Dopo aver apportato queste modifiche, la sintassi dichiarativa di FormView s dovrebbe essere simile al seguente:


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample6.aspx)]

Successivamente, creare un gestore eventi per s FormView `ItemCommand` evento. In questo gestore eventi è necessario innanzitutto determinare se interrompere tutti i prodotti è stato fatto clic sul pulsante. Se in tal caso, si vuole creare un'istanza del `ProductsBLL` classe e richiamare il `DiscontinueAllProductsForSupplier(supplierID)` metodo passando la `SupplierID` del controllo FormView selezionato:


[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample7.vb)]

Si noti che il `SupplierID` del fornitore corrente selezionato in FormView è possibile accedere tramite il s FormView [ `SelectedValue` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formview.selectedvalue.aspx). Il `SelectedValue` proprietà restituisce il primo della chiave dati valore per il record viene visualizzato in FormView. S FormView [ `DataKeyNames` proprietà](https://msdn.microsoft.com/system.web.ui.webcontrols.formview.datakeynames.aspx), che indica i dati di campi da cui valori di chiave i dati vengono estratti da, è stata impostata automaticamente su `SupplierID` da Visual Studio quando si associano ObjectDataSource nella parte posteriore FormView nel passaggio 2.

Con il `ItemCommand` gestore dell'evento creato, è opportuno testare la pagina. Individuare il Quesos de Cooperativa fornitore 'Las Cabras' (è s fornitore quinto in FormView per me). Il fornitore fornisce due prodotti, Queso Cabrales e Queso Manchego La Pastora, costituiti entrambi da *non* non più disponibile.

Si supponga che la Germania Cooperativa Quesos 'Las Cabras' esce dall'azienda e quindi i prodotti non viene più utilizzata. Scegliere il pulsante di tutti i prodotti di interrompere. Verrà visualizzata la finestra di dialogo di conferma sul lato client (vedere Figura 16).


[![Cooperativa de Quesos Las Cabras fornisce due prodotti attivi](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image43.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image42.png)

**Figura 16**: Cooperativa de Quesos Las Cabras fornisce due prodotti attivi ([fare clic per visualizzare l'immagine ingrandita](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image44.png))


Se si fa clic su OK nella finestra di dialogo Conferma sul lato client, verrà eseguito l'invio del modulo, provocando un postback in cui s FormView `ItemCommand` viene generato l'evento. Il gestore dell'evento è stato creato verrà quindi eseguito, richiamare il `DiscontinueAllProductsForSupplier(supplierID)` metodo e sospensione Queso Cabrales sia Queso Manchego La Pastora prodotti.

Se è disabilitata, lo stato di visualizzazione GridView s GridView è essere riassociati nell'archivio dati sottostante a ogni postback e pertanto verrà immediatamente aggiornato per riflettere che questi due prodotti sono più utilizzati (vedere Figura 17). Se, tuttavia, non è stato disabilitato lo stato di visualizzazione in GridView, è necessario riassociare manualmente i dati a GridView dopo aver apportato questa modifica. A tale scopo, è sufficiente effettuare una chiamata a GridView s `DataBind()` metodo subito dopo la chiamata di `DiscontinueAllProductsForSupplier(supplierID)` metodo.


[![Dopo il pulsante interrompere tutti i prodotti, i prodotti fornitore s vengono aggiornati di conseguenza](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image46.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image45.png)

**Figura 17**: dopo il pulsante interrompere tutti i prodotti, i prodotti fornitore s vengono aggiornati di conseguenza ([fare clic per visualizzare l'immagine ingrandita](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image47.png))


## <a name="step-6-creating-an-updateproduct-overload-in-the-business-logic-layer-for-adjusting-a-product-s-price"></a>Passaggio 6: Creazione di un Overload UpdateProduct nel livello di logica di Business per la regolazione di un prezzo del prodotto s

Come interrompere tutti i prodotti pulsante in FormView, per aggiungere i pulsanti per aumentare e ridurre il prezzo di un prodotto in GridView è necessario innanzitutto aggiungere i metodi appropriati di livello di accesso ai dati e il livello di logica di Business. Poiché è già disponibile un metodo che aggiorna una riga DAL singolo prodotto, offriamo tale funzionalità creando un nuovo overload per il `UpdateProduct` metodo BLL.

Il nostro passato `UpdateProduct` overload aver acquisito in una combinazione di campi prodotto come valori scalari di input e quindi aggiornare solo i campi per il prodotto specificato. Per questo overload viene leggermente diverso da questo standard e invece di passare all'interno del prodotto s `ProductID` e la percentuale in base a cui si desidera modificare il `UnitPrice` (anziché il passaggio di nuovo, regolato `UnitPrice` stesso). Questo approccio semplificherà il codice è necessario scrivere nella classe code-behind pagina ASP.NET, poiché non abbiamo necessario preoccuparsi di determinazione del prodotto corrente s t `UnitPrice`.

Il `UpdateProduct` overload per questa esercitazione viene illustrata di seguito:


[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample8.vb)]

Questo overload consente di recuperare informazioni relative al prodotto specificato tramite i dispositivi DAL `GetProductByProductID(productID)` metodo. Verifica quindi se il prodotto s `UnitPrice` viene assegnato un database `NULL` valore. Questo caso, il prezzo viene lasciato inalterato. Se, tuttavia, esiste un non -`NULL` `UnitPrice` valore, il metodo aggiorna il prodotto s `UnitPrice` per la percentuale specificata (`unitPriceAdjustmentPercent`).

## <a name="step-7-adding-the-increase-and-decrease-buttons-to-the-gridview"></a>Passaggio 7: Aggiungere i pulsanti di riduzione e un aumento GridView

Il GridView e DetailsView, sono entrambi costituiti da una raccolta di campi. Oltre a BoundField CheckBoxFields e TemplateFields, ASP.NET include il ButtonField, che, come suggerisce il nome, viene visualizzato come una colonna con un pulsante, LinkButton o ImageButton per ogni riga. Simile al controllo FormView, fare clic su *qualsiasi* pulsante all'interno di GridView pulsanti di paging, modifica o eliminazione pulsanti, pulsanti di ordinamento e così via provoca un postback e genera gli oggetti di GridView [ `RowCommand` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowcommand.aspx).

Il ButtonField ha un `CommandName` proprietà che assegna il valore specificato per ognuno dei relativi pulsanti `CommandName` proprietà. Come con FormView, il `CommandName` valore viene utilizzato il `RowCommand` gestore eventi per determinare quale pulsante è stato fatto clic.

S consentono di aggiungere due ButtonFields di nuovo a GridView, uno con un testo del pulsante Price + 10% e l'altro con il testo prezzo -10%. Per aggiungere questi ButtonFields, fare clic sul collegamento Modifica colonne dallo smart tag GridView s, selezionare il tipo di campo ButtonField dall'elenco in alto a sinistra e fare clic sul pulsante Aggiungi.


![Aggiungere due ButtonFields al controllo GridView.](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image48.png)

**Figura 18**: aggiungere due ButtonFields al controllo GridView.


Spostare le due ButtonFields in modo che vengano visualizzati come i primi due campi di GridView. Successivamente, impostare il `Text` le proprietà di questi due ButtonFields prezzo + 10% a e prezzo -10% e `CommandName` proprietà IncreasePrice e DecreasePrice, rispettivamente. Per impostazione predefinita, un ButtonField come la relativa colonna di pulsanti LinkButton. Questo può essere modificato, tuttavia, tramite il s ButtonField [ `ButtonType` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.buttonfieldbase.buttontype.aspx). Consente di disporre di questi due ButtonFields sottoposto a rendering come pulsanti normali; s Pertanto, impostare il `ButtonType` proprietà `Button`. La figura 19 mostra i campi la finestra di dialogo dopo avere apportate queste modifiche; Dopo che è il markup dichiarativo s GridView.


![Configurare il testo ButtonFields CommandName e proprietà ButtonType](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image49.png)

**Figura 19**: configurare il ButtonFields `Text`, `CommandName`, e `ButtonType` proprietà


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample9.aspx)]

Con questi ButtonFields creato, il passaggio finale consiste nel creare un gestore eventi per i dispositivi di GridView `RowCommand` evento. Questo gestore eventi, se è stata attivata perché il prezzo + 10 pulsanti di prezzo -10% o % stato fatto clic, è necessario determinare la `ProductID` per la riga il cui tipo di pulsante è stato fatto clic e quindi richiamare il `ProductsBLL` classe s `UpdateProduct` , passando in appropriata `UnitPrice` Rettifica percentuale insieme al `ProductID`. Il codice seguente esegue queste attività:

[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample10.vb)]

Per determinare il `ProductID` per la riga il cui prezzo + 10 è stato scelto % o % prezzo -10, è necessario consultare s GridView `DataKeys` insieme. Questa raccolta contiene i valori dei campi specificati nel `DataKeyNames` proprietà per ogni riga GridView. Poiché i dispositivi di GridView `DataKeyNames` è stata impostata su ProductID da Visual Studio durante l'associazione ObjectDataSource GridView, `DataKeys(rowIndex).Value` fornisce il `ProductID` per l'oggetto specificato *rowIndex*.

Il ButtonField passa automaticamente il *rowIndex* della riga il cui pulsante selezionato tramite il `e.CommandArgument` parametro. Pertanto, per determinare il `ProductID` per la riga il cui prezzo + 10 è stato scelto % o % prezzo -10, utilizziamo: `Convert.ToInt32(SuppliersProducts.DataKeys(Convert.ToInt32(e.CommandArgument)).Value)`.

Come con il pulsante di sospendere tutti i prodotti, se è stata disabilitata lo stato di visualizzazione s GridView, GridView è essere riassociati nell'archivio dati sottostante a ogni postback e pertanto verrà immediatamente aggiornato per riflettere la modifica di un prezzo che si verifica da scegliere uno dei pulsanti. Se, tuttavia, non è stato disabilitato lo stato di visualizzazione in GridView, è necessario riassociare manualmente i dati a GridView dopo aver apportato questa modifica. A tale scopo, è sufficiente effettuare una chiamata a GridView s `DataBind()` metodo subito dopo la chiamata di `UpdateProduct` metodo.

Figura 20 viene mostrata la pagina quando si visualizzano i prodotti forniti da nonna Kelly Homestead. Figura 21 Mostra i risultati dopo il prezzo + 10% pulsante è stato fatto clic due volte per Boysenberry Spread della nonna e il pulsante di prezzo -10% di una volta per birra irlandese.


[![GridView include Price + 10% e prezzo -10% pulsanti](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image51.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image50.png)

**Figura 20**: il prezzo comprende GridView + 10% e prezzo -10% pulsanti ([fare clic per visualizzare l'immagine ingrandita](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image52.png))


[![I prezzi per il prodotto primo e la terzo sono stati aggiornati tramite il prezzo + 10% e prezzo -10% pulsanti](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image54.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image53.png)

**Figura 21**: il prezzi per il primo e il terzo prodotto sono stati aggiornati tramite il prezzo + 10% e prezzo -10% pulsanti ([fare clic per visualizzare l'immagine ingrandita](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image55.png))


> [!NOTE]
> Il GridView e DetailsView, può essere ImageButtons aggiunto al loro TemplateFields, LinkButton o pulsanti. Come con BoundField, questi pulsanti, quando si fa clic, causerà un postback, la generazione di GridView s `RowCommand` evento. Quando aggiunta di pulsanti in un TemplateField, tuttavia, il pulsante s `CommandArgument` non viene impostata automaticamente per l'indice della riga di quando utilizzare ButtonFields. Se è necessario determinare l'indice di riga del pulsante su cui è stato fatto clic all'interno di `RowCommand` gestore eventi, è necessario impostare manualmente il pulsante s `CommandArgument` proprietà nella relativa sintassi dichiarativa all'interno di TemplateField, utilizzando codice simile:  
> `<asp:Button runat="server" ... CommandArgument='<%# CType(Container, GridViewRow).RowIndex %>' />`.


## <a name="summary"></a>Riepilogo

I controlli GridView, DetailsView e FormView possono includere pulsanti, LinkButton o ImageButtons. Questi pulsanti, quando si fa clic, provocare un postback e generare il `ItemCommand` eventi nei controlli del controllo FormView e DetailsView e `RowCommand` evento in GridView. Questi controlli Web di dati sono una funzionalità integrata per gestire le azioni comuni relative ai comandi, ad esempio l'eliminazione o modifica di record. Tuttavia, è anche possibile utilizzare i pulsanti che, quando si fa clic, rispondere con l'esecuzione di codice personalizzato.

A tale scopo, è necessario creare un gestore eventi per il `ItemCommand` o `RowCommand` evento. In questo gestore eventi controllare innanzitutto in ingresso `CommandName` valore per determinare il pulsante scelto e quindi intraprendere l'azione personalizzata appropriata. In questa esercitazione è stato illustrato come utilizzare i pulsanti e ButtonFields per interrompere tutti i prodotti per un fornitore specificato o per aumentare o diminuire il prezzo di un prodotto specifico del 10%.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Precedente](adding-and-responding-to-buttons-to-a-gridview-cs.md)
