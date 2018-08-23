---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-across-two-pages-vb
title: Applicazione di filtri su due pagine (VB) master/dettaglio | Microsoft Docs
author: rick-anderson
description: In questa esercitazione implementeremo questo modello usando un controllo GridView per elencare i fornitori nel database. Ogni riga del fornitore in GridView conterrà un Vie...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 361d6a44-3f1f-4daf-85df-d4c2b8bf065d
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-across-two-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 290c4eba6f77a6006d424c3f05b77c1c128026b3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823937"
---
<a name="masterdetail-filtering-across-two-pages-vb"></a>Applicazione di filtri su due pagine (VB) master/dettaglio
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'App di esempio](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_9_VB.exe) o [Scarica il PDF](master-detail-filtering-across-two-pages-vb/_static/datatutorial09vb1.pdf)

> In questa esercitazione implementeremo questo modello usando un controllo GridView per elencare i fornitori nel database. Ogni riga del fornitore in GridView conterrà un collegamento di visualizzare i prodotti che, quando selezionato, richiederà all'utente di una pagina separata che elenca i prodotti per il fornitore selezionato.


## <a name="introduction"></a>Introduzione

Nelle due esercitazioni precedenti è stato illustrato come visualizzare i report master o di dettaglio in una singola pagina web con controlli DropDownList per visualizzare i record di "master" e un [GridView](master-detail-filtering-with-a-dropdownlist-vb.md) oppure [DetailsView](master-detail-filtering-with-two-dropdownlists-vb.md) controllo per visualizzare il " Dettagli." Un altro modello comune utilizzato per report dettagli/master deve avere i record master in una pagina web e i dettagli mostrati in un altro. Un sito Web forum, ad esempio [i forum ASP.NET](https://forums.asp.net/), è un ottimo esempio di questo modello in pratica. Forum di ASP.NET sono costituiti da un'ampia gamma di forum Introduzione a Web Form, controlli di presentazione dei dati e così via. Ogni forum è costituito da più thread e ogni thread è costituito da un numero di post. Nella home page dei forum di ASP.NET, sono elencati i forum. Facendo clic su un forum indirizzati per un `ShowForum.aspx` pagina nella quale sono elencati i thread del forum. Analogamente, facendo clic su un thread si viene indirizzati al `ShowPost.aspx`, che consente di visualizzare i post per il thread che è stato fatto clic.

In questa esercitazione implementeremo questo modello usando un controllo GridView per elencare i fornitori nel database. Ogni riga del fornitore in GridView conterrà un collegamento di visualizzare i prodotti che, quando selezionato, richiederà all'utente di una pagina separata che elenca i prodotti per il fornitore selezionato.

## <a name="step-1-addingsupplierlistmasteraspxandproductsforsupplierdetailsaspxpages-to-thefilteringfolder"></a>Passaggio 1: Aggiunta`SupplierListMaster.aspx`e`ProductsForSupplierDetails.aspx`delle pagine di`Filtering`cartella

Quando si definisce il layout di pagina nella terza esercitazione abbiamo aggiunto un numero di pagine "starter" i `BasicReporting`, `Filtering`, e `CustomFormatting` cartelle. Tuttavia, viene non è stata aggiunta una pagina iniziale per questa esercitazione in quel momento, pertanto si consiglia di aggiungere due nuove pagine per il `Filtering` cartella: `SupplierListMaster.aspx` e `ProductsForSupplierDetails.aspx`. `SupplierListMaster.aspx` Elenca i record "master" (i fornitori) mentre `ProductsForSupplierDetails.aspx` visualizzerà i prodotti per il fornitore selezionato.

Quando la creazione di queste due nuove pagine essere determinati da associare con la `Site.master` pagina master.


![Aggiungere tutte le pagine ProductsForSupplierDetails.aspx SupplierListMaster.aspx nella cartella filtro](master-detail-filtering-across-two-pages-vb/_static/image1.png)

**Figura 1**: aggiungere i `SupplierListMaster.aspx` e `ProductsForSupplierDetails.aspx` delle pagine di `Filtering` cartella


Inoltre, quando si aggiungono nuove pagine al progetto, assicurarsi di aggiornare il file di mappa del sito, `Web.sitemap`, di conseguenza. Per questa esercitazione è sufficiente aggiungere il `SupplierListMaster.aspx` pagina per la mappa del sito usando il contenuto XML seguente come figlio di Filtering Reports `<siteMapNode>` elemento:


[!code-xml[Main](master-detail-filtering-across-two-pages-vb/samples/sample1.xml)]

> [!NOTE]
> Consente di automatizzare il processo di aggiornamento file di mappa del sito durante l'aggiunta di nuovo ASP.NET le pagine usando [K. Scott Allen](http://odetocode.com/Blogs/scott/)del gratuito Visual Studio [Macro della mappa del sito](http://odetocode.com/Blogs/scott/archive/2005/11/29/2537.aspx).


## <a name="step-2-displaying-the-supplier-list-insupplierlistmasteraspx"></a>Passaggio 2: Visualizzare l'elenco di fornitori in`SupplierListMaster.aspx`

Con il `SupplierListMaster.aspx` e `ProductsForSupplierDetails.aspx` pagine create, il passaggio successivo consiste nel creare il controllo GridView di fornitori in `SupplierListMaster.aspx`. Aggiungere un controllo GridView alla pagina e associarlo a un nuovo oggetto ObjectDataSource. ObjectDataSource deve usare la `SuppliersBLL` della classe `GetSuppliers()` per restituire tutti i suoi fornitori.


[![Selezionare la classe SuppliersBLL](master-detail-filtering-across-two-pages-vb/_static/image3.png)](master-detail-filtering-across-two-pages-vb/_static/image2.png)

**Figura 2**: selezionare il `SuppliersBLL` classe ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-across-two-pages-vb/_static/image4.png))


[![Configurare ObjectDataSource per usare il metodo GetSuppliers()](master-detail-filtering-across-two-pages-vb/_static/image6.png)](master-detail-filtering-across-two-pages-vb/_static/image5.png)

**Figura 3**: configurare ObjectDataSource per usare il `GetSuppliers()` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-across-two-pages-vb/_static/image7.png))


È necessario includere un collegamento denominato vista prodotti in ogni riga GridView che, quando si fa clic, viene visualizzato il `ProductsForSupplierDetails.aspx` passando la riga selezionata `SupplierID` valore tramite la stringa di query. Ad esempio, se l'utente fa clic sul collegamento Visualizza i prodotti per il fornitore di commercianti di Tokyo (che dispone di un `SupplierID` valore pari a 4), devono essere inviati a `ProductsForSupplierDetails.aspx?SupplierID=4`.

A tale scopo, aggiungere un [HyperLinkField](https://msdn.microsoft.com/library/system.web.ui.webcontrols.hyperlinkfield.aspx) per GridView, che aggiunge un collegamento ipertestuale per ogni riga GridView. Avviare facendo clic sul collegamento Modifica colonne dallo smart tag del controllo GridView. Successivamente, selezionare il HyperLinkField dall'elenco in alto a sinistra e fare clic su Aggiungi per includere il HyperLinkField nell'elenco dei campi del controllo GridView.


[![Aggiungere un HyperLinkField a GridView](master-detail-filtering-across-two-pages-vb/_static/image9.png)](master-detail-filtering-across-two-pages-vb/_static/image8.png)

**Figura 4**: aggiungere un HyperLinkField a GridView ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-across-two-pages-vb/_static/image10.png))


Il HyperLinkField può essere configurato per usare lo stesso testo o URL valori il collegamento in ogni riga GridView oppure è possibile basare tali valori sui valori di dati associati a ogni riga. Per specificare un valore statico valore per tutte le righe, usare il HyperLinkField `Text` o `NavigateUrl` proprietà. Poiché si desidera che il testo del collegamento per essere uguale per tutte le righe, impostare il HyperLinkField `Text` proprietà per visualizzare i prodotti.


[![Impostare proprietà Text dell'HyperLinkField per visualizzare i prodotti](master-detail-filtering-across-two-pages-vb/_static/image12.png)](master-detail-filtering-across-two-pages-vb/_static/image11.png)

**Figura 5**: impostare il HyperLinkField `Text` proprietà per visualizzare i prodotti ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-across-two-pages-vb/_static/image13.png))


Per impostare il testo o valori di URL deve essere basato sui dati sottostanti associati alla riga GridView, specificare i dati di campi di testo o valori di URL devono essere recuperati nel `DataTextField` o `DataNavigateUrlFields` proprietà. `DataTextField` può essere impostato solo per un singolo campo di dati; `DataNavigateUrlFields`, tuttavia, può essere impostato su un elenco delimitato da virgole dei campi dati. È spesso necessario basare l'URL su una combinazione del valore del campo dati della riga corrente e alcuni tag statico o il testo. In questa esercitazione, ad esempio, si vuole che l'URL dei collegamenti dell'HyperLinkField `ProductsForSupplierDetails.aspx?SupplierID=supplierID`, dove *`supplierID`* è della riga di ogni GridView `SupplierID` valore. Si noti che è necessario sia statici e guidate dai dati valori qui: il `ProductsForSupplierDetails.aspx?SupplierID=` parte dell'URL del collegamento è statico, mentre la *`supplierID`* parte è basato sui dati come relativo valore è ogni riga `SupplierID` valore.

Per indicare una combinazione di valori statici e guidate dai dati, usare il `DataTextFormatString` e `DataNavigateUrlFormatString` proprietà. In queste proprietà immettere il markup statico in base alle esigenze e quindi usare il marcatore `{0}` in cui si desidera il valore del campo specificato nel `DataTextField` o `DataNavigateUrlFields` proprietà da visualizzare. Se il `DataNavigateUrlFields` proprietà ha più campi specificati `{0}` in cui si desidera che il primo valore del campo inserito, `{1}` per il secondo valore del campo e così via.

Si applica a seguire l'esercitazione, è necessario impostare il `DataNavigateUrlFields` proprietà `SupplierID`, dal momento che è il campo dati il cui valore è necessario personalizzare in base per ogni riga e il `DataNavigateUrlFormatString` proprietà `ProductsForSupplierDetails.aspx?SupplierID={0}`.


[![Configurare il HyperLinkField per includere l'URL del collegamento appropriato in base all'ID fornitore](master-detail-filtering-across-two-pages-vb/_static/image15.png)](master-detail-filtering-across-two-pages-vb/_static/image14.png)

**Figura 6**: configurare il HyperLinkField per includere il corretto collegamento URL in base al momento il `SupplierID` ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-across-two-pages-vb/_static/image16.png))


Dopo aver aggiunto il HyperLinkField, è possibile personalizzare e riordinare i campi del controllo GridView. Il markup seguente mostra il controllo GridView dopo che sono state apportate alcune personalizzazioni a livello di campo secondario.


[!code-aspx[Main](master-detail-filtering-across-two-pages-vb/samples/sample2.aspx)]

Si consiglia di visualizzare il `SupplierListMaster.aspx` pagina tramite un browser. Come illustrato nella figura 7, la pagina attualmente Elenca tutti i fornitori con un collegamento di visualizzare i prodotti. Facendo clic su Visualizza prodotti collegamento verrà visualizzata `ProductsForSupplierDetails.aspx`, passando lungo del fornitore `SupplierID` nella stringa di query.


[![Ogni riga Supplier contiene un collegamento di prodotti di visualizzazione](master-detail-filtering-across-two-pages-vb/_static/image18.png)](master-detail-filtering-across-two-pages-vb/_static/image17.png)

**Figura 7**: ogni riga Supplier contiene un collegamento di prodotti di visualizzazione ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-across-two-pages-vb/_static/image19.png))


## <a name="step-3-listing-the-suppliers-products-inproductsforsupplierdetailsaspx"></a>Passaggio 3: Elenco di prodotti del fornitore`ProductsForSupplierDetails.aspx`

A questo punto il `SupplierListMaster.aspx` pagina sta inviando agli utenti di `ProductsForSupplierDetails.aspx`, passando un fornitore selezionato `SupplierID` nella stringa di query. Passaggio finale dell'esercitazione consiste nel visualizzare i prodotti in un controllo GridView nella `ProductsForSupplierDetails.aspx` la cui `SupplierID` è uguale al `SupplierID` passato tramite la stringa di query. Per completare questa Guida di avvio tramite l'aggiunta di un controllo GridView per il `ProductsForSupplierDetails.aspx` pagina, usando un nuovo controllo ObjectDataSource denominato `ProductsBySupplierDataSource` che richiama la `GetProductsBySupplierID(supplierID)` metodo dal `ProductsBLL` classe.


[![Aggiungere un nuovo oggetto ObjectDataSource denominato ProductsBySupplierDataSource](master-detail-filtering-across-two-pages-vb/_static/image21.png)](master-detail-filtering-across-two-pages-vb/_static/image20.png)

**Figura 8**: aggiungere un nuovo oggetto ObjectDataSource denominato `ProductsBySupplierDataSource` ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-across-two-pages-vb/_static/image22.png))


[![Selezionare la classe ProductsBLL](master-detail-filtering-across-two-pages-vb/_static/image24.png)](master-detail-filtering-across-two-pages-vb/_static/image23.png)

**Figura 9**: selezionare il `ProductsBLL` classe ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-across-two-pages-vb/_static/image25.png))


[![Avere ObjectDataSource richiama il metodo GetProductsBySupplierID(supplierID)](master-detail-filtering-across-two-pages-vb/_static/image27.png)](master-detail-filtering-across-two-pages-vb/_static/image26.png)

**Figura 10**: il richiamare l'oggetto ObjectDataSource il `GetProductsBySupplierID(supplierID)` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-across-two-pages-vb/_static/image28.png))


Il passaggio finale della procedura guidata Configura origine dati viene chiesto di specificare per fornire l'origine del `GetProductsBySupplierID(supplierID)` del metodo *`supplierID`* parametro. Per usare il valore di stringa di query, impostare l'origine del parametro di stringa di query e immettere il nome del valore della stringa di query da usare nella casella di testo QueryStringField (`SupplierID`).


[![Popolare il valore del parametro dal valore Querystring SupplierID supplierID](master-detail-filtering-across-two-pages-vb/_static/image30.png)](master-detail-filtering-across-two-pages-vb/_static/image29.png)

**Figura 11**: popolare le *`supplierID`* valore del parametro dal `SupplierID` valore Querystring ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-across-two-pages-vb/_static/image31.png))


Questo è tutto qui! Figura 12 vengono illustrate le `ProductsForSupplierDetails.aspx` pagina quando visitati, selezionando il collegamento di commercianti Tokyo da `SupplierListMaster.aspx`.


[![Vengono visualizzati i prodotti forniti dagli operatori Tokyo](master-detail-filtering-across-two-pages-vb/_static/image33.png)](master-detail-filtering-across-two-pages-vb/_static/image32.png)

**Figura 12**: vengono visualizzati il prodotti forniti dagli operatori Tokyo ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-across-two-pages-vb/_static/image34.png))


## <a name="displaying-supplier-information-inproductsforsupplierdetailsaspx"></a>Visualizzazione di informazioni sui fornitori in`ProductsForSupplierDetails.aspx`

Come illustrato nella figura 12, il `ProductsForSupplierDetails.aspx` pagina elenca semplicemente i prodotti offerti dal `SupplierID` specificato nella stringa di query. Qualcuno ha inviato direttamente a questa pagina, tuttavia, non avrebbe saputo che mostra i prodotti di commercianti Tokyo figura 12. Per risolvere il problema che è possibile visualizzare le informazioni sui fornitori in questa pagina.

Iniziare aggiungendo un controllo FormView sopra i prodotti GridView. Creare un nuovo controllo ObjectDataSource denominato `SuppliersDataSource` che richiama la `SuppliersBLL` della classe `GetSupplierBySupplierID(supplierID)` (metodo).


[![Selezionare la classe SuppliersBLL](master-detail-filtering-across-two-pages-vb/_static/image36.png)](master-detail-filtering-across-two-pages-vb/_static/image35.png)

**Figura 13**: selezionare il `SuppliersBLL` classe ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-across-two-pages-vb/_static/image37.png))


[![Avere ObjectDataSource richiama il metodo GetSupplierBySupplierID(supplierID)](master-detail-filtering-across-two-pages-vb/_static/image39.png)](master-detail-filtering-across-two-pages-vb/_static/image38.png)

**Figura 14**: il richiamare l'oggetto ObjectDataSource il `GetSupplierBySupplierID(supplierID)` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-across-two-pages-vb/_static/image40.png))


Come con le `ProductsBySupplierDataSource`, hanno il *`supplierID`* parametro assegnato il valore della `SupplierID` valore querystring.


[![Popolare il valore del parametro dal valore Querystring SupplierID supplierID](master-detail-filtering-across-two-pages-vb/_static/image42.png)](master-detail-filtering-across-two-pages-vb/_static/image41.png)

**Figura 15**: popolare le *`supplierID`* valore del parametro dal `SupplierID` valore Querystring ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-across-two-pages-vb/_static/image43.png))


Quando si associano FormView a ObjectDataSource nella visualizzazione progettazione, Visual Studio creerà automaticamente il controllo FormView `ItemTemplate`, `InsertItemTemplate`, e `EditItemTemplate` con controlli Label e TextBox Web per ognuno dei campi di dati restituiti dal ObjectDataSource. Poiché si vuole solo visualizzare supplier informazioni è possibile rimuovere il `InsertItemTemplate` e `EditItemTemplate`. Successivamente, Modifica ItemTemplate, in modo che venga visualizzato il nome della società del fornitore in un `<h3>` elemento e l'indirizzo, città, paese e numero di telefono di sotto il nome della società. In alternativa, è possibile impostare manualmente il controllo FormView `DataSourceID` e creare le `ItemTemplate` markup, come abbiamo fatto nel "[la visualizzazione dei dati con ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md)" esercitazione.

Dopo tali modifiche al markup dichiarativo del controllo FormView dovrebbe essere simile al seguente:


[!code-aspx[Main](master-detail-filtering-across-two-pages-vb/samples/sample3.aspx)]

Figura 16 vengono illustrate una cattura di schermata del `ProductsForSupplierDetails.aspx` dopo che le informazioni di fornitori come indicate in precedenza sono stato incluse.


[![L'elenco dei prodotti sono inclusi un riepilogo relativo al fornitore](master-detail-filtering-across-two-pages-vb/_static/image45.png)](master-detail-filtering-across-two-pages-vb/_static/image44.png)

**Figura 16**: l'elenco dei prodotti sono inclusi un riepilogo sul fornitore ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-across-two-pages-vb/_static/image46.png))


## <a name="applying-the-final-touches-for-theproductsforsupplierdetailsaspxui"></a>Applicazione finale tocca per la`ProductsForSupplierDetails.aspx`dell'interfaccia utente

Per migliorare l'utente esperienza per questo report sono disponibili un paio di aggiunte si deve apportare al `ProductsForSupplierDetails.aspx` pagina. Attualmente l'unico modo un utente può passare dalla `ProductsForSupplierDetails.aspx` pagina tornare all'elenco di fornitori consiste nel fare clic sul pulsante Indietro del browser. È possibile aggiungere un controllo collegamento ipertestuale per la `ProductsForSupplierDetails.aspx` pagina che consente di tornare al `SupplierListMaster.aspx`, fornendo un altro modo l'utente deve tornare all'elenco master.


[![Aggiungere un controllo collegamento ipertestuale che indirizza l'utente torna a SupplierListMaster.aspx](master-detail-filtering-across-two-pages-vb/_static/image48.png)](master-detail-filtering-across-two-pages-vb/_static/image47.png)

**Figura 17**: aggiungere un controllo collegamento ipertestuale per l'utente torna a sfruttare `SupplierListMaster.aspx` ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-across-two-pages-vb/_static/image49.png))


Se l'utente fa clic sul collegamento Visualizza i prodotti per un fornitore che non esiste alcun prodotto, il `ProductsBySupplierDataSource` ObjectDataSource in `ProductsForSupplierDetails.aspx` non verrà restituito alcun risultato. GridView associato a ObjectDataSource non eseguire il rendering di eventuale markup risultante in un'area vuota nella pagina nel browser dell'utente. Per maggiore chiarezza comunicare all'utente che non sono presenti prodotti associati con il fornitore selezionato è possibile impostare il controllo GridView `EmptyDataText` proprietà al messaggio di cui si desidera venga visualizzati quando si verifica una situazione di questo. Ho impostato questa proprietà su "Non sono presenti prodotti forniti da questo fornitore"

Per impostazione predefinita, tutti i fornitori nel database di Employees fornire almeno un prodotto. Per questa esercitazione, tuttavia, è stata modificata manualmente il `Products` tabella in modo che il fornitore Escargots informazioni non è più associato alcun prodotto. Figura 18 Mostra la pagina dei dettagli per informazioni Escargots dopo che è stata apportata questa modifica.


[![Gli utenti vengono informati che il fornitore non fornisce alcun prodotto](master-detail-filtering-across-two-pages-vb/_static/image51.png)](master-detail-filtering-across-two-pages-vb/_static/image50.png)

**Figura 18**: gli utenti vengono informati che il fornitore non fornisce alcun prodotto ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-across-two-pages-vb/_static/image52.png))


## <a name="summary"></a>Riepilogo

Mentre i report master o di dettaglio è possono visualizzare i record master e di dettaglio in una singola pagina, in molti siti Web sono suddivisi in due pagine web. In questa esercitazione è stato illustrato come implementare questo rapporto master/dettaglio facendo in modo che i fornitori elencati in un controllo GridView nella pagina web "master" e i prodotti associati elencati nella pagina "Dettagli". Ogni riga del fornitore nella pagina master web contenuti un collegamento alla pagina dei dettagli passato lungo la riga `SupplierID` valore. Tali collegamenti specifiche della riga possono essere facilmente aggiunto usando HyperLinkField di GridView.

Nella pagina dei dettagli del recupero di tali prodotti per il fornitore specificato era eseguito richiamando il `ProductsBLL` della classe `GetProductsBySupplierID(supplierID)` (metodo). Il *`supplierID`* valore del parametro è stato specificato in modo dichiarativo utilizzando il parametro querystring come origine del parametro. È stato anche illustrato come visualizzare i dettagli del fornitore nella pagina dei dettagli con un controllo FormView.

Nostri [prossima esercitazione](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md) è quella finale sui report master o di dettaglio. Esamineremo come visualizzare un elenco di prodotti in un controllo GridView in cui ogni riga ha un pulsante di selezione. Facendo clic sul pulsante Seleziona verranno visualizzati i dettagli del prodotto in un controllo DetailsView nella stessa pagina.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Hilton Giesenow, revisore per questa esercitazione. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](master-detail-filtering-with-two-dropdownlists-vb.md)
> [Successivo](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md)
