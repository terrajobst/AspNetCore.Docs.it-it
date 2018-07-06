---
uid: web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-vb
title: Aggiunta e risposta ai pulsanti in un controllo GridView (VB) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione verrà esaminato come aggiungere pulsanti personalizzati, sia a un modello e ai campi di un controllo GridView o DetailsView. In particolare, ti invieremo un numero di comp...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: 06c6bbd2-4bdc-435b-87a3-df2c868f4baa
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-vb
msc.type: authoredcontent
ms.openlocfilehash: 458f90bf70d6f10402583623ef62bf1572040ce4
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365058"
---
<a name="adding-and-responding-to-buttons-to-a-gridview-vb"></a>Aggiunta e risposta ai pulsanti in un controllo GridView (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'App di esempio](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_28_VB.exe) o [Scarica il PDF](adding-and-responding-to-buttons-to-a-gridview-vb/_static/datatutorial28vb1.pdf)

> In questa esercitazione verrà esaminato come aggiungere pulsanti personalizzati, sia a un modello e ai campi di un controllo GridView o DetailsView. In particolare, verrà compilata un'interfaccia che dispone di un controllo FormView che consente all'utente di spostarsi tra i fornitori.


## <a name="introduction"></a>Introduzione

Sebbene molti scenari di creazione report comportano l'accesso di sola lettura ai dati del report, è s non è insolito per i report includano la possibilità di eseguire azioni in base ai dati visualizzati. In genere questo ha richiesto l'aggiunta di un controllo pulsante, LinkButton e ImageButton Web con ogni record visualizzato nel report che, quando si fa clic, causa un postback e richiama un codice lato server. Modificare ed eliminare i dati in un record per record di base è l'esempio più comune. In realtà, come abbiamo visto inizia con la [Panoramica di inserimento, aggiornamento ed eliminazione di dati](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) esercitazione, modifica ed eliminazione è talmente comune che i controlli GridView, DetailsView e FormView possono supportare tale funzionalità senza il necessario per la scrittura di una singola riga di codice.

Per modificare ed eliminare i pulsanti, il controllo GridView, DetailsView e FormView inoltre controlli possono includere anche i pulsanti, i controlli LinkButton o ImageButtons che, quando si fa clic, eseguire una logica personalizzata lato server. In questa esercitazione verrà esaminato come aggiungere pulsanti personalizzati, sia a un modello e ai campi di un controllo GridView o DetailsView. In particolare, verrà compilata un'interfaccia che dispone di un controllo FormView che consente all'utente di spostarsi tra i fornitori. Per un determinato fornitore, FormView conterranno le informazioni sul fornitore insieme a un controllo pulsante Web che, se si fa clic, contrassegna tutti i relativi prodotti associati come non più disponibile. Inoltre, un controllo GridView sono elencati i prodotti specificati dal fornitore selezionato, con ogni riga che contiene il prezzo di aumentare e sconto sul prezzo pulsanti che, se si fa clic, aumentare o ridurre il prodotto s `UnitPrice` 10% (vedere la figura 1).


[![FormView e GridView contengono i pulsanti per eseguono azioni personalizzate](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image2.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image1.png)

**Figura 1**: entrambi FormView e GridView contengono pulsanti che eseguire azioni personalizzate ([fare clic per visualizzare l'immagine con dimensioni normali](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image3.png))


## <a name="step-1-adding-the-button-tutorial-web-pages"></a>Passaggio 1: Aggiunta di pagine Web di esercitazione pulsante

Prima di esaminare come aggiungere un pulsanti personalizzati, consentire s prima di tutto si consiglia di creare le pagine ASP.NET nel progetto di sito Web che è necessario per questa esercitazione. Iniziare aggiungendo una nuova cartella denominata `CustomButtons`. Successivamente, aggiungere le seguenti due pagine ASP.NET per quella cartella, assicurandosi di associare ogni pagina con il `Site.master` pagina master:

- `Default.aspx`
- `CustomButtons.aspx`


![Aggiungere le pagine ASP.NET per le esercitazioni relative ai pulsanti personalizzate](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image4.png)

**Figura 2**: aggiungere le pagine ASP.NET per le esercitazioni relative ai pulsanti personalizzate


In altre cartelle, analogo a `Default.aspx` nella `CustomButtons` cartella elencherà le esercitazioni nella relativa sezione. Si tenga presente che il `SectionLevelTutorialListing.ascx` controllo utente fornisce questa funzionalità. Pertanto, aggiungere questo controllo utente da `Default.aspx` trascinandolo da Esplora soluzioni nella pagina di visualizzazione della struttura s.


[![Aggiungere il controllo utente sectionleveltutoriallisting. ascx a default. aspx](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image6.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image5.png)

**Figura 3**: aggiungere i `SectionLevelTutorialListing.ascx` controllo utente da `Default.aspx` ([fare clic per visualizzare l'immagine con dimensioni normali](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image7.png))


Infine, aggiungere le pagine come voci per il `Web.sitemap` file. In particolare, aggiungere il markup seguente dopo il Paging e ordinamento `<siteMapNode>`:


[!code-xml[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample1.xml)]

Dopo aver aggiornato `Web.sitemap`, si consiglia di visualizzare il sito Web di esercitazioni tramite un browser. Il menu a sinistra ora include elementi per la modifica, inserimento ed eliminazione di esercitazioni.


![Mappa del sito include ora la voce per l'esercitazione di pulsanti personalizzati](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image8.png)

**Figura 4**: la mappa del sito include ora la voce per l'esercitazione di pulsanti personalizzati


## <a name="step-2-adding-a-formview-that-lists-the-suppliers"></a>Passaggio 2: Aggiunta di un controllo FormView che elenca i fornitori

Let s iniziare con questa esercitazione aggiungendo FormView che elenca i fornitori. Come descritto nell'introduzione, questo controllo FormView consentirà all'utente di spostarsi tra i fornitori, che mostra i prodotti specificati dal fornitore in un controllo GridView. Inoltre, questo controllo FormView includerà un pulsante che, quando si fa clic, contrassegna tutti i prodotti di fornitore s come non più disponibile. Prima che si sono relative a noi con il pulsante personalizzato l'aggiunta a FormView, consentire s prima di tutto creare semplicemente FormView in modo che vengano visualizzate le informazioni del fornitore.

Iniziare aprendo il `CustomButtons.aspx` nella pagina di `CustomButtons` cartella. Aggiungere un controllo FormView alla pagina trascinandolo dalla casella degli strumenti nella finestra di progettazione e set relativo `ID` proprietà `Suppliers`. Controllo FormView s nello smart tag, scegliere di creare un nuovo oggetto ObjectDataSource denominato `SuppliersDataSource`.


[![Creare un nuovo oggetto ObjectDataSource denominato SuppliersDataSource](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image10.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image9.png)

**Salvare opzione (nessuno) come un database **valore, è necessario restituire il** gestore dell'evento.


Configurare il nuovo oggetto ObjectDataSource in modo che viene eseguita una query dal `SuppliersBLL` classe s `GetSuppliers()` (metodo) (vedere la figura 6). Poiché questo controllo FormView non fornisce un'interfaccia per l'aggiornamento delle informazioni sul fornitore, selezionare opzione (nessuno) nell'elenco a discesa nella scheda aggiornamenti.


[![Configurare l'origine dati per usare la classe SuppliersBLL s GetSuppliers() (metodo)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image13.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image12.png)

**Figura 6**: configurare l'origine dati da usare il `SuppliersBLL` classe s `GetSuppliers()` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image14.png))


Dopo la configurazione di ObjectDataSource, Visual Studio genera una `InsertItemTemplate`, `EditItemTemplate`, e `ItemTemplate` per FormView. Rimuovere il `InsertItemTemplate` e `EditItemTemplate` e modificare il `ItemTemplate` in modo che venga visualizzato solo il fornitore s aziendale nome e numero di telefono. Infine, attivare il supporto di paging per il controllo FormView selezionando la casella di controllo Attiva Paging dal suo smart tag (o tramite l'impostazione relativa `AllowPaging` proprietà `True`). Dopo tali modifiche markup dichiarativo s pagina dovrebbe essere simile al seguente:


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample2.aspx)]

Figura 7 mostra la pagina CustomButtons.aspx quando viene visualizzato tramite un browser.


[![FormView sono elencati i CompanyName e i campi telefono dal fornitore attualmente selezionato](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image16.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image15.png)

**Figura 7**: Elenca il FormView la `CompanyName` e `Phone` campi dal fornitore attualmente selezionato ([fare clic per visualizzare l'immagine con dimensioni normali](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image17.png))


## <a name="step-3-adding-a-gridview-that-lists-the-selected-supplier-s-products"></a>Passaggio 3: Aggiunta di un controllo GridView in cui sono elencati i prodotti di fornitore selezionato s

Prima di aggiungere interrompere tutti i prodotti pulsante al modello s di FormView, facciamo s aggiungere un controllo GridView di sotto di FormView che elenca i prodotti specificati dal fornitore selezionato. Per eseguire questa operazione, aggiungere un controllo GridView alla pagina, impostare relativi `ID` proprietà `SuppliersProducts`, e aggiungere un nuovo oggetto ObjectDataSource denominato `SuppliersProductsDataSource`.


[![Creare un nuovo oggetto ObjectDataSource denominato SuppliersProductsDataSource](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image19.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image18.png)

**Figura 8**: creare un nuovo oggetto ObjectDataSource denominato `SuppliersProductsDataSource` ([fare clic per visualizzare l'immagine con dimensioni normali](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image20.png))


Configurare ObjectDataSource per usare la classe ProductsBLL s `GetProductsBySupplierID(supplierID)` (metodo) (vedere la figura 9). Anche se questo controllo GridView verranno consentono un prezzo del prodotto s affinché venga regolato, ha vinto t utilizzeremo incorporati di modifica o l'eliminazione di funzionalità da GridView. Pertanto, è possibile impostare l'elenco a discesa su (nessuno) per s ObjectDataSource schede UPDATE, INSERT e DELETE.


[![Configurare l'origine dati per usare la classe ProductsBLL s GetProductsBySupplierID(supplierID) (metodo)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image22.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image21.png)

**Figura 9**: configurare l'origine dati da usare il `ProductsBLL` classe s `GetProductsBySupplierID(supplierID)` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image23.png))


Poiché il `GetProductsBySupplierID(supplierID)` metodo accetta un parametro di input, la procedura guidata ObjectDataSource richiede per l'origine di questo valore del parametro. Passare il `SupplierID` valore di FormView, impostare l'elenco di riepilogo a discesa Origine parametro al controllo e l'elenco di riepilogo a discesa ControlID a `Suppliers` (l'ID del controllo FormView creato nel passaggio 2).


[![Indicare che supplierID parametro deve provenire dal controllo FormView Suppliers](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image25.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image24.png)

**Figura 10**: indicano che il *`supplierID`* parametro deve provenire dal `Suppliers` controllo FormView ([fare clic per visualizzare l'immagine con dimensioni normali](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image26.png))


Dopo aver completato la procedura guidata ObjectDataSource, GridView conterrà un BoundField o CampoCasellaDiControllo per ognuno dei campi dati di s del prodotto. Let s ridurre questo per mostrare solo il `ProductName` e `UnitPrice` BoundField insieme al `Discontinued` CampoCasellaDiControllo; consentono, inoltre, il formato s il `UnitPrice` BoundField in modo che il testo viene formattato come una valuta. I GridView e `SuppliersProductsDataSource` markup dichiarativo di ObjectDataSource s dovrebbe essere simile al markup seguente:


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample3.aspx)]

A questo punto seguire l'esercitazione consente di visualizzare un report dettagli/master, consentendo all'utente di scegliere un fornitore di FormView nella parte superiore e per visualizzare i prodotti offerti da tale fornitore tramite il controllo GridView nella parte inferiore. Figura 11 mostra una cattura di schermata della pagina quando si seleziona il fornitore di commercianti Tokyo da FormView.


[![I prodotti s fornitore selezionato vengono visualizzati in GridView](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image28.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image27.png)

**Figura 11**: il fornitore selezionato s prodotti vengono visualizzati in GridView ([fare clic per visualizzare l'immagine con dimensioni normali](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image29.png))


## <a name="step-4-creating-dal-and-bll-methods-to-discontinue-all-products-for-a-supplier"></a>Passaggio 4: Creazione di DAL e BLL metodi per interrompere tutti i prodotti per un fornitore

Prima di poter aggiungere un pulsante a FormView che, quando si fa clic, interrompe tutti i prodotti di fornitore s, prima è necessario aggiungere un metodo a sia DAL e BLL che esegue questa azione. In particolare, questo metodo verrà denominato `DiscontinueAllProductsForSupplier(supplierID)`. Quando si fa clic sul pulsante FormView s, si sarà richiamare questo metodo nel livello di logica di Business, passando di fornitore selezionato s `SupplierID`; il livello BLL chiamerà quindi verso il basso per il metodo di livello di accesso ai dati corrispondente, che genererà un `UPDATE` dell'istruzione il database interrompe i prodotti di fornitore specificato s.

Come è stato eseguito nelle esercitazioni precedenti, si userà un approccio di dal basso in alto, a partire da creando il metodo DAL, quindi il metodo BLL e infine l'implementazione della funzionalità in una pagina ASP.NET. Aprire il `Northwind.xsd` DataSet tipizzata nel `App_Code/DAL` cartella e aggiungere un nuovo metodo per il `ProductsTableAdapter` (fare clic sul `ProductsTableAdapter` e scegliere Aggiungi Query). In questo modo verrà visualizzata la configurazione guidata Query TableAdapter, quale attraverso il processo di aggiunta del nuovo metodo. Per iniziare, che indica che il metodo DAL utilizzerà un'istruzione SQL ad hoc.


[![Creare il metodo DAL utilizzando un'istruzione SQL Ad Hoc](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image31.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image30.png)

**Figura 12**: creare il metodo DAL usando un'istruzione SQL Ad Hoc ([fare clic per visualizzare l'immagine con dimensioni normali](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image32.png))


Successivamente, verrà richiesto di Stati Uniti per quanto riguarda il tipo di query da creare. Poiché il `DiscontinueAllProductsForSupplier(supplierID)` metodo sarà necessario aggiornare il `Products` tabella di database, l'impostazione di `Discontinued` campo su 1 per tutti i prodotti forniti dall'oggetto specificato *`supplierID`*, è necessario creare una query che aggiorna i dati.


[![Scegliere il tipo di Query UPDATE](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image34.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image33.png)

**Figura 13**: scegliere il tipo di Query di aggiornamento ([fare clic per visualizzare l'immagine con dimensioni normali](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image35.png))


La schermata successiva della procedura fornisce s TableAdapter esistente `UPDATE` istruzione che aggiorna ogni campo definito nel `Products` DataTable. Sostituire il testo della query con l'istruzione seguente:


[!code-sql[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample4.sql)]

Dopo aver immesso la query e facendo clic su Avanti, l'ultima schermata della procedura guidata chiede il nuovo nome del metodo s usare `DiscontinueAllProductsForSupplier`. Completare la procedura guidata facendo clic sul pulsante Fine. Quando si torna alla finestra di Progettazione DataSet verrà visualizzato un nuovo metodo nella `ProductsTableAdapter` denominato `DiscontinueAllProductsForSupplier(@SupplierID)`.


[![Denominare il nuovo DiscontinueAllProductsForSupplier DAL metodo](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image37.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image36.png)

**Figura 14**: nome del nuovo metodo DAL `DiscontinueAllProductsForSupplier` ([fare clic per visualizzare l'immagine con dimensioni normali](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image38.png))


Con il `DiscontinueAllProductsForSupplier(supplierID)` metodo creato nel livello di accesso ai dati, l'attività successiva consiste nel creare il `DiscontinueAllProductsForSupplier(supplierID)` metodo nel livello di logica di Business. A tale scopo, aprire il `ProductsBLL` file di classe e aggiungere quanto segue:


[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample5.vb)]

Questo metodo chiama semplicemente verso il basso per il `DiscontinueAllProductsForSupplier(supplierID)` metodo di DAL, passando l'oggetto fornito *`supplierID`* valore del parametro. Se si sono verificati eventuali regole di business che è consentito solo un fornitore prodotti s essere sospeso in determinate circostanze, tali regole devono essere implementate in questo caso, il livello BLL.

> [!NOTE]
> A differenza di `UpdateProduct` esegue l'overload nel `ProductsBLL` (classe), il `DiscontinueAllProductsForSupplier(supplierID)` firma del metodo non include il `DataObjectMethodAttribute` attributo (`<System.ComponentModel.DataObjectMethodAttribute(System.ComponentModel.DataObjectMethodType.Update, Boolean)>`). Ciò preclude la `DiscontinueAllProductsForSupplier(supplierID)` metodo dall'elenco a discesa s s procedura guidata Configura origine dati ObjectDataSource nella scheda aggiornamenti. Ho ve omesso l'attributo perché verrà chiamato il `DiscontinueAllProductsForSupplier(supplierID)` metodo direttamente da un gestore eventi nella nostra pagina ASP.NET.


## <a name="step-5-adding-a-discontinue-all-products-button-to-the-formview"></a>Passaggio 5: Aggiunta di un interrompere tutti i prodotti pulsante a FormView

Con il `DiscontinueAllProductsForSupplier(supplierID)` BLL e DAL metodo completato, il passaggio finale per l'aggiunta la possibilità di sospendere tutti i prodotti per il fornitore selezionato consiste nell'aggiungere un controllo pulsante Web per i dispositivi di FormView `ItemTemplate`. Let s aggiungere questi pulsanti sotto il numero di telefono supplier s con il testo del pulsante, interrompere tutti i prodotti e un `ID` valore della proprietà `DiscontinueAllProductsForSupplier`. È possibile aggiungere questo controllo pulsante Web tramite la finestra di progettazione facendo clic sul collegamento Modifica modelli nello smart tag FormView s (vedere la figura 15) o direttamente tramite la sintassi dichiarativa.


[![Aggiungere un'interruzione di tutti i prodotti Web pulsante all'ItemTemplate s FormView](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image40.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image39.png)

**Figura 15**: aggiungere interrompere tutti i prodotti Web un pulsante al s FormView `ItemTemplate` ([fare clic per visualizzare l'immagine con dimensioni normali](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image41.png))


Quando fa clic sul pulsante per visitare un utente la pagina, un postback previsioni e i dispositivi di FormView [ `ItemCommand` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formview.itemcommand.aspx) viene attivato. Per eseguire codice personalizzato in risposta a questo pulsante viene fatto clic, è possibile creare un gestore eventi per questo evento. Capire, tuttavia, che il `ItemCommand` ogni volta che viene generato un evento *qualsiasi* si fa clic sul controllo pulsante, LinkButton e ImageButton Web all'interno di FormView. Ciò significa che quando l'utente si sposti da uno pagina a altro in FormView, il `ItemCommand` viene generato l'evento; stessa operazione quando l'utente fa clic su Nuovo, modificare o eliminare in un controllo FormView che supporta l'inserimento, aggiornamento o eliminazione.

Poiché il `ItemCommand` viene attivato indipendentemente dal fatto che pulsante, nel caso in cui gestore di è necessario un modo per determinare se interrompere tutti i prodotti è stato fatto clic sul pulsante o se si tratta di un altro pulsante. A tale scopo, è possibile impostare il controllo pulsante Web s `CommandName` proprietà su un valore di identificazione. Quando fa clic sul pulsante, ciò `CommandName` valore viene passato il `ItemCommand` gestore eventi, ci permette di determinare se interrompere tutti i prodotti pulsante è stato fatto clic sul pulsante. Impostare la s interrompere tutti i prodotti pulsante `CommandName` proprietà DiscontinueProducts.

Infine, consentire s di usare una finestra di dialogo di conferma dal lato client per assicurarsi che l'utente desideri interrompere i prodotti di fornitore selezionato s. Come abbiamo visto nel [aggiunta di conferma dal lato Client quando Elimina](../editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-vb.md) dell'esercitazione, questo può essere eseguito con un frammento di codice JavaScript. In particolare, impostare la proprietà di OnClientClick s controllo Web pulsante su `return confirm('This will mark _all_ of this supplier\'s products as discontinued. Are you certain you want to do this?');`

Dopo aver apportato queste modifiche, la sintassi dichiarativa s FormView dovrebbe essere simile al seguente:


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample6.aspx)]

Successivamente, creare un gestore eventi per s FormView `ItemCommand` evento. In questo gestore eventi è necessario innanzitutto determinare se interrompere tutti i prodotti è stato fatto clic sul pulsante. Se pertanto si desidera creare un'istanza del `ProductsBLL` classe e richiamare relativi `DiscontinueAllProductsForSupplier(supplierID)` metodo, passando il `SupplierID` FormView selezionato:


[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample7.vb)]

Si noti che il `SupplierID` del fornitore corrente selezionato nella FormView sono accessibili con dispositivi FormView [ `SelectedValue` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formview.selectedvalue.aspx). Il `SelectedValue` proprietà restituisce il primo valore chiave dati per il record viene visualizzato nel controllo FormView. Le s FormView [ `DataKeyNames` proprietà](https://msdn.microsoft.com/system.web.ui.webcontrols.formview.datakeynames.aspx), che indica i dati di campi da cui i valori chiave di dati vengono estratti da, è stata impostata automaticamente su `SupplierID` da Visual Studio quando si associano ObjectDataSource nella parte posteriore FormView nel passaggio 2.

Con la `ItemCommand` gestore dell'evento creato, si consiglia di testare la pagina. Individuare il Quesos de Cooperativa supplier 'Las Cabras' (lo s quinto fornitore in FormView per me). Il fornitore offre due prodotti, Queso Cabrales e Queso Manchego La Pastora, entrambi *non* non più disponibile.

Si supponga che Cooperativa Germania Quesos 'Las Cabras' esce dall'azienda e pertanto i propri prodotti devono essere sospeso. Fare clic su di interrompere tutti i prodotti pulsante. Verrà visualizzata la finestra di dialogo di conferma dal lato client (vedere la figura 16).


[![Cooperativa de Quesos Las Cabras fornisce due prodotti attivi](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image43.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image42.png)

**Figura 16**: Cooperativa de Quesos Las Cabras fornisce due prodotti attivi ([fare clic per visualizzare l'immagine con dimensioni normali](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image44.png))


Se si fa clic su OK nella finestra di dialogo di conferma dal lato client, l'invio del modulo procederà, causando un postback in cui i dispositivi di FormView `ItemCommand` viene generato l'evento. Il gestore dell'evento è stato creato verrà quindi eseguito, richiamo il `DiscontinueAllProductsForSupplier(supplierID)` metodo e discontinue Queso Cabrales sia Queso Manchego La Pastora prodotti.

Se è stato disabilitato lo stato di visualizzazione GridView s, il controllo GridView è in corso riassociata nell'archivio dati sottostante a ogni postback e pertanto verrà immediatamente aggiornato per riflettere che questi due prodotti sono ora non più disponibili (vedere Figura 17). Se, tuttavia, non è stato disabilitato lo stato di visualizzazione in GridView, dovrai associare di nuovo manualmente i dati a GridView dopo aver apportato questa modifica. A tale scopo, è sufficiente effettuare una chiamata a s GridView `DataBind()` metodo immediatamente dopo aver richiamato il `DiscontinueAllProductsForSupplier(supplierID)` (metodo).


[![Dopo il pulsante interrompere tutti i prodotti, i prodotti di fornitore s vengono aggiornati di conseguenza](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image46.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image45.png)

**Figura 17**: dopo il pulsante interrompere tutti i prodotti vengono aggiornati di conseguenza i prodotti di fornitore s ([fare clic per visualizzare l'immagine con dimensioni normali](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image47.png))


## <a name="step-6-creating-an-updateproduct-overload-in-the-business-logic-layer-for-adjusting-a-product-s-price"></a>Passaggio 6: Creazione di un Overload UpdateProduct nel livello di logica di Business per la regolazione di un prezzo per prodotto

Ad esempio con interrompere tutti i prodotti pulsante in FormView, per aggiungere i pulsanti per aumentare e ridurre il prezzo per un prodotto in GridView è necessario innanzitutto aggiungere i metodi di livello di accesso ai dati e Business Logic Layer appropriati. Perché è già disponibile un metodo che aggiorna una riga DAL singolo prodotto, è possibile fornire tale funzionalità mediante la creazione di un nuovo overload per il `UpdateProduct` metodo nel livello BLL.

Il passato `UpdateProduct` overload è stato acquisito in una combinazione di campi di prodotto come valori di input scalari e sono quindi aggiornati solo i campi per il prodotto specificato. Per questo overload verrà leggermente diversi da questo standard e passa invece all'interno del prodotto s `ProductID` e la percentuale in base a cui si desidera modificare il `UnitPrice` (invece di passare di nuovo, regolato `UnitPrice` stesso). Questo approccio semplificherà il codice è necessario scrivere nella classe code-behind della pagina ASP.NET, poiché non abbiamo non è necessario preoccuparsi di determinare il prodotto corrente s `UnitPrice`.

Il `UpdateProduct` overload per questa esercitazione viene illustrata di seguito:


[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample8.vb)]

Questo overload consente di recuperare informazioni relative al prodotto specificato tramite i dispositivi DAL `GetProductByProductID(productID)` (metodo). Verifica quindi se il prodotto 1!s `UnitPrice` viene assegnato un database `NULL` valore. Se si tratta, il prezzo viene lasciato inalterato. Se, tuttavia, esiste un non -`NULL` `UnitPrice` valore, il metodo aggiorna il prodotto s `UnitPrice` base alla percentuale specificata (`unitPriceAdjustmentPercent`).

## <a name="step-7-adding-the-increase-and-decrease-buttons-to-the-gridview"></a>Passaggio 7: Aggiungere i pulsanti di riduzione e aumento a GridView

Il controllo GridView e DetailsView, sono entrambi costituiti da una raccolta di campi. Oltre a BoundField CheckBoxFields e TemplateFields, ASP.NET include la ButtonField, che, come suggerisce il nome, viene rappresentato come una colonna con un pulsante, LinkButton e ImageButton per ogni riga. Simile a FormView, facendo clic *eventuali* pulsante all'interno di GridView i pulsanti di spostamento, modifica o eliminazione pulsanti, pulsanti di ordinamento e così via determina un postback e genera la s GridView [ `RowCommand` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowcommand.aspx).

Il ButtonField ha un `CommandName` proprietà che assegna il valore specificato per ognuno dei relativi pulsanti `CommandName` proprietà. Come con FormView, il `CommandName` valore viene usato dal `RowCommand` gestore dell'evento per determinare quale pulsante è stato fatto clic.

Let s aggiungere due nuove ButtonFields a GridView, uno con un testo del pulsante Price + 10% e l'altro con il testo del prezzo da -10%. Per aggiungere questi ButtonFields, fare clic sul collegamento Modifica colonne dello smart tag s GridView, selezionare il tipo di campo ButtonField dall'elenco in alto a sinistra e fare clic sul pulsante Aggiungi.


![Aggiungere due ButtonFields a GridView](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image48.png)

**Figura 18**: aggiungere due ButtonFields a GridView


Spostare il due ButtonFields, in modo che vengano visualizzati come i primi due campi di GridView. Successivamente, impostare il `Text` le proprietà di questi due ButtonFields a + 10% dei prezzi e prezzo -10% e il `CommandName` proprietà IncreasePrice e DecreasePrice, rispettivamente. Per impostazione predefinita, un ButtonField come la relativa colonna di pulsanti LinkButton. Ciò può essere modificato, tuttavia, tramite la s ButtonField [ `ButtonType` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.buttonfieldbase.buttontype.aspx). Let s hanno questi due ButtonFields sottoposto a rendering come pulsanti regolari push; Pertanto, impostare il `ButtonType` proprietà `Button`. Figura 19 mostra i campi nella finestra di dialogo dopo aver apportate queste modifiche; Dopo che è il markup dichiarativo s GridView.


![Configurare il testo ButtonFields CommandName e proprietà ButtonType](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image49.png)

**Figura 19**: configurare il ButtonFields `Text`, `CommandName`, e `ButtonType` proprietà


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample9.aspx)]

Con queste ButtonFields creato, il passaggio finale consiste nel creare un gestore eventi per s GridView `RowCommand` evento. Questo gestore eventi, se è stata attivata perché il prezzo + 10 sono stati visitato i pulsanti di prezzo tra -10% o %, deve determinare il `ProductID` per la riga sul cui pulsante è stato fatto clic e quindi richiamare il `ProductsBLL` classe s `UpdateProduct` , passando il `UnitPrice` regolazione percentuale insieme al `ProductID`. Il codice seguente esegue queste attività:

[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample10.vb)]

Per determinare il `ProductID` per la riga il cui prezzo + 10 è stato fatto clic sul pulsante di prezzo tra -10% o %, è necessario consultare la s GridView `DataKeys` raccolta. Questa raccolta contiene i valori dei campi specificati nel `DataKeyNames` proprietà per ogni riga GridView. Poiché i dispositivi di GridView `DataKeyNames` proprietà è stata impostata su ProductID da Visual Studio durante l'associazione ObjectDataSource di GridView `DataKeys(rowIndex).Value` fornisce il `ProductID` per l'oggetto specificato *rowIndex*.

Il ButtonField passa automaticamente il *rowIndex* della riga di cui è stato fatto clic sul cui pulsante tramite il `e.CommandArgument` parametro. Pertanto, per determinare il `ProductID` per la riga il cui prezzo + 10 è stato fatto clic sul pulsante di prezzo tra -10% o %, usiamo: `Convert.ToInt32(SuppliersProducts.DataKeys(Convert.ToInt32(e.CommandArgument)).Value)`.

Come con il pulsante di interrompere tutti i prodotti, se è stato disabilitato lo stato di visualizzazione GridView s, il controllo GridView è in corso riassociata nell'archivio dati sottostante a ogni postback e pertanto verrà immediatamente aggiornato per riflettere una variazione di prezzo che viene eseguita da facendo clic su uno dei pulsanti. Se, tuttavia, non è stato disabilitato lo stato di visualizzazione in GridView, dovrai associare di nuovo manualmente i dati a GridView dopo aver apportato questa modifica. A tale scopo, è sufficiente effettuare una chiamata a s GridView `DataBind()` metodo immediatamente dopo aver richiamato il `UpdateProduct` (metodo).

Figura 20 è illustrata la pagina quando si visualizzano i prodotti offerti da nonna Kelly Homestead. Figura 21 vengono illustrati i risultati dopo il prezzo + 10% pulsante è stato fatto clic due volte per Boysenberry Spread della nonna e il pulsante di prezzo tra -10% di una volta per irlandese birra.


[![Il controllo GridView include Price + 10% e i pulsanti di prezzo tra -10%](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image51.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image50.png)

**Figura 20**: il prezzo include GridView + 10% e prezzo -10% pulsanti ([fare clic per visualizzare l'immagine con dimensioni normali](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image52.png))


[![I prezzi per il primo e il terzo di prodotto sono stati aggiornati tramite il prezzo + 10% e i pulsanti di prezzo tra -10%](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image54.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image53.png)

**Figura 21**: il prezzo per il primo e il terzo prodotto sono stati aggiornati tramite il prezzo + 10% e prezzo -10% pulsanti ([fare clic per visualizzare l'immagine con dimensioni normali](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image55.png))


> [!NOTE]
> Il controllo GridView e DetailsView, può anche avere pulsanti, i controlli LinkButton o ImageButtons aggiunto al loro TemplateFields. Come con i BoundField, questi pulsanti, quando si fa clic, causerà un postback, che genera la s GridView `RowCommand` evento. Quando aggiunta di pulsanti in un TemplateField, tuttavia, il pulsante s `CommandArgument` non viene impostata automaticamente per l'indice della riga come succede quando si usa ButtonFields. Se è necessario determinare l'indice di riga del pulsante su cui è stato fatto clic all'interno di `RowCommand` gestore eventi, è necessario impostare manualmente il pulsante s `CommandArgument` proprietà nella relativa sintassi dichiarativa all'interno di TemplateField, usando codice simile:  
> `<asp:Button runat="server" ... CommandArgument='<%# CType(Container, GridViewRow).RowIndex %>' />`.


## <a name="summary"></a>Riepilogo

Possono includere i controlli GridView, DetailsView e FormView tutti i pulsanti, i controlli LinkButton o ImageButtons. Questi pulsanti, quando si fa clic, possono causare un postback e generare il `ItemCommand` eventi nei controlli DetailsView e FormView e `RowCommand` evento nel GridView. Questi controlli Web dei dati dispongono di funzionalità incorporate per gestire operazioni frequenti relative ai comandi, ad esempio l'eliminazione o modifica di record. Tuttavia, è anche possibile usare i pulsanti che, quando si fa clic, rispondere con l'esecuzione del nostro codice personalizzato.

A tale scopo, è necessario creare un gestore eventi per il `ItemCommand` o `RowCommand` evento. In questo gestore dell'evento, controlliamo dapprima in ingresso `CommandName` valore per determinare quale pulsante è stato fatto clic, quindi intraprendere l'azione personalizzata appropriata. In questa esercitazione è stato illustrato come usare i pulsanti e ButtonFields per interrompere tutti i prodotti per un fornitore specificato o per aumentare o diminuire il prezzo di un prodotto specifico del 10%.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Precedente](adding-and-responding-to-buttons-to-a-gridview-cs.md)
