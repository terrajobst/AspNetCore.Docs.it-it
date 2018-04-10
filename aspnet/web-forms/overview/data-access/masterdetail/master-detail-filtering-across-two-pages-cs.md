---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-across-two-pages-cs
title: Master-Details filtri tra due pagine (c#) | Documenti Microsoft
author: rick-anderson
description: In questa esercitazione è viene implementato questo modello utilizzando un oggetto GridView per elencare i fornitori nel database. Ogni riga del fornitore in GridView conterrà un Vie...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 552d2d50-fe73-4153-9a7f-2b379bec4625
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-across-two-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 0858bf9c8dc380898647293825145654ac1dbc34
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="masterdetail-filtering-across-two-pages-c"></a>Master-Details filtri tra due pagine (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare App di esempio](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_9_CS.exe) o [Scarica il PDF](master-detail-filtering-across-two-pages-cs/_static/datatutorial09cs1.pdf)

> In questa esercitazione è viene implementato questo modello utilizzando un oggetto GridView per elencare i fornitori nel database. Ogni riga del fornitore in GridView conterrà un collegamento Visualizza i prodotti che, quando è selezionato, richiederà all'utente di una pagina separata che elenca i prodotti per il fornitore selezionato.


## <a name="introduction"></a>Introduzione

Nelle due esercitazioni precedenti abbiamo visto come [visualizzare report master/dettaglio in una singola pagina web utilizzando controlli DropDownList](master-detail-filtering-with-a-dropdownlist-cs.md) a [visualizzare i record "master" e un controllo GridView o DetailsView](master-detail-filtering-with-two-dropdownlists-cs.md) per visualizzare il " Dettagli". Un altro modello comune usato per i report master/dettagli è che i record master in una pagina web e i dettagli visualizzati in un altro. Un sito Web forum, ad esempio [forum di ASP.NET](https://forums.asp.net/), è un ottimo esempio di questo modello in pratica. Forum di ASP.NET sono costituiti da una varietà di operazioni preliminari, forum Web Form, controlli di presentazione di dati e così via. Ogni forum è costituito da più thread e ogni thread è costituito da un numero di inserimenti. Nella home page dei forum di ASP.NET, vengono elencati i forum. Facendo clic su un forum indirizzati di un `ShowForum.aspx` pagina nella quale sono elencati i thread del forum. Analogamente, facendo clic su un thread necessario per `ShowPost.aspx`, che consente di visualizzare i post per il thread su cui è stato fatto clic.

In questa esercitazione è viene implementato questo modello utilizzando un oggetto GridView per elencare i fornitori nel database. Ogni riga del fornitore in GridView conterrà un collegamento Visualizza i prodotti che, quando è selezionato, richiederà all'utente di una pagina separata che elenca i prodotti per il fornitore selezionato.

## <a name="step-1-addingsupplierlistmasteraspxandproductsforsupplierdetailsaspxpages-to-thefilteringfolder"></a>Passaggio 1: Aggiunta di`SupplierListMaster.aspx`e`ProductsForSupplierDetails.aspx`delle pagine di`Filtering`cartella

Durante la definizione del layout di pagina nella terza esercitazione è stato aggiunto un numero di pagine "starter" il `BasicReporting`, `Filtering`, e `CustomFormatting` cartelle. Tuttavia, si non aggiungere una pagina iniziale per questa esercitazione in quel momento, pertanto è opportuno aggiungere due nuove pagine per il `Filtering` cartella: `SupplierListMaster.aspx` e `ProductsForSupplierDetails.aspx`. `SupplierListMaster.aspx` verranno elencati i record "master" (i fornitori) durante `ProductsForSupplierDetails.aspx` visualizzerà i prodotti per il fornitore selezionato.

Quando la creazione di due nuove pagine essere determinati da associare con la `Site.master` pagina master.


![Aggiungere tutte le pagine ProductsForSupplierDetails.aspx SupplierListMaster.aspx nella cartella filtro](master-detail-filtering-across-two-pages-cs/_static/image1.png)

**Figura 1**: aggiungere il `SupplierListMaster.aspx` e `ProductsForSupplierDetails.aspx` delle pagine di `Filtering` cartella


Inoltre, quando si aggiungono nuove pagine al progetto, assicurarsi di aggiornare il file di mappa del sito, `Web.sitemap`, di conseguenza. Per questa esercitazione, è sufficiente aggiungere il `SupplierListMaster.aspx` pagina mappa del sito utilizzando il seguente contenuto XML come elemento figlio dei report filtro `<siteMapNode>` elemento:


[!code-xml[Main](master-detail-filtering-across-two-pages-cs/samples/sample1.xml)]

> [!NOTE]
> Consente di automatizzare il processo di aggiornamento del file di mappa del sito quando aggiunta ASP.NET nuove pagine utilizzando [K. Scott Allen](http://odetocode.com/Blogs/scott/)del gratuita di Visual Studio [Macro della mappa del sito](http://odetocode.com/Blogs/scott/archive/2005/11/29/2537.aspx).


## <a name="step-2-displaying-the-supplier-list-insupplierlistmasteraspx"></a>Passaggio 2: Visualizzare l'elenco di fornitori in`SupplierListMaster.aspx`

Con il `SupplierListMaster.aspx` e `ProductsForSupplierDetails.aspx` pagine create, nel passaggio successivo consiste nella creazione dei fornitori in GridView `SupplierListMaster.aspx`. Aggiungere un controllo GridView alla pagina e associarlo a un nuovo oggetto ObjectDataSource. ObjectDataSource deve utilizzare il `SuppliersBLL` della classe `GetSuppliers()` per restituire tutti i fornitori.


[![Selezionare la classe SuppliersBLL](master-detail-filtering-across-two-pages-cs/_static/image3.png)](master-detail-filtering-across-two-pages-cs/_static/image2.png)

**Figura 2**: selezionare il `SuppliersBLL` classe ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-across-two-pages-cs/_static/image4.png))


[![Configurare ObjectDataSource per utilizzare il metodo GetSuppliers()](master-detail-filtering-across-two-pages-cs/_static/image6.png)](master-detail-filtering-across-two-pages-cs/_static/image5.png)

**Figura 3**: configurare ObjectDataSource per utilizzare il `GetSuppliers()` metodo ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-across-two-pages-cs/_static/image7.png))


È necessario includere un collegamento denominato Visualizza i prodotti in ogni riga GridView che, quando si fa clic, richiede all'utente di `ProductsForSupplierDetails.aspx` passando la riga selezionata `SupplierID` valore tramite la stringa di query. Ad esempio, se l'utente fa clic sul collegamento Visualizza i prodotti per il fornitore Tokyo Traders (che presenta un `SupplierID` valore pari a 4), devono essere inviati a `ProductsForSupplierDetails.aspx?SupplierID=4`.

A tale scopo, aggiungere un [HyperLinkField](https://msdn.microsoft.com/library/system.web.ui.webcontrols.hyperlinkfield.aspx) a GridView, che aggiunge un collegamento ipertestuale per ogni riga GridView. Avviare facendo clic sul collegamento Modifica colonne smart tag del controllo GridView. Successivamente, selezionare il HyperLinkField dall'elenco in alto a sinistra e fare clic su Aggiungi per includere il HyperLinkField nell'elenco di campi del controllo GridView.


[![Aggiungere un HyperLinkField al controllo GridView.](master-detail-filtering-across-two-pages-cs/_static/image9.png)](master-detail-filtering-across-two-pages-cs/_static/image8.png)

**Figura 4**: aggiungere un HyperLinkField a GridView ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-across-two-pages-cs/_static/image10.png))


Il HyperLinkField può essere configurato per utilizzare lo stesso testo o i valori di collegamento in ogni riga GridView URL o possibile basare i valori dei dati associati a ogni riga di questi valori. Per specificare un valore statico valore per tutte le righe, utilizzare il HyperLinkField `Text` o `NavigateUrl` proprietà. Poiché si desidera che il testo del collegamento siano gli stessi per tutte le righe, impostare il HyperLinkField `Text` proprietà per visualizzare i prodotti.


[![Impostare proprietà Text del HyperLinkField per visualizzare i prodotti](master-detail-filtering-across-two-pages-cs/_static/image12.png)](master-detail-filtering-across-two-pages-cs/_static/image11.png)

**Figura 5**: impostare il HyperLinkField `Text` proprietà per visualizzare i prodotti ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-across-two-pages-cs/_static/image13.png))


Per impostare i valori di testo o URL deve essere basato sui dati sottostanti associati alla riga GridView, specificare i dati di campi di testo o valori di URL devono essere recuperati nel `DataTextField` o `DataNavigateUrlFields` proprietà. `DataTextField` può essere impostato solo su un singolo campo dati; `DataNavigateUrlFields`, tuttavia, possibile specificare un elenco delimitato da virgole dei campi dati. È spesso necessario basare il testo o l'URL su una combinazione del valore del campo dati della riga corrente e del markup statico. In questa esercitazione, ad esempio, si vuole che l'URL dei collegamenti dell'HyperLinkField da `ProductsForSupplierDetails.aspx?SupplierID=supplierID`, dove *`supplierID`* della riga di ogni GridView `SupplierID` valore. Si noti che è necessario sia statici e guidate dai dati i valori qui: il `ProductsForSupplierDetails.aspx?SupplierID=` parte dell'URL del collegamento è statico, mentre il *`supplierID`* parte è guidata dai dati perché il relativo valore è ogni riga `SupplierID` valore.

Per indicare una combinazione di valori statici e guidate dai dati, utilizzare il `DataTextFormatString` e `DataNavigateUrlFormatString` proprietà. Queste proprietà immettere il markup statico in base alle esigenze e quindi utilizzare il marcatore `{0}` in cui si desidera il valore del campo specificato nel `DataTextField` o `DataNavigateUrlFields` proprietà da visualizzare. Se il `DataNavigateUrlFields` proprietà utilizza più campi specificati `{0}` dove si desidera il primo valore del campo inserito, `{1}` per il secondo valore del campo e così via.

Applicare questa esercitazione, è necessario impostare il `DataNavigateUrlFields` proprietà `SupplierID`, dal momento che è il campo dei dati il cui valore è necessario personalizzare i singoli per ogni riga, e `DataNavigateUrlFormatString` proprietà `ProductsForSupplierDetails.aspx?SupplierID={0}`.


[![Configurare il HyperLinkField per includere l'URL del collegamento appropriato in base al SupplierID](master-detail-filtering-across-two-pages-cs/_static/image15.png)](master-detail-filtering-across-two-pages-cs/_static/image14.png)

**Figura 6**: configurare HyperLinkField per includere il corretto collegamento URL in base al momento il `SupplierID` ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-across-two-pages-cs/_static/image16.png))


Dopo aver aggiunto il HyperLinkField, è possibile personalizzare e riordinare i campi del controllo GridView. Di seguito viene illustrato il controllo GridView dopo che sono state apportate alcune personalizzazioni a livello di campo limitate.


[!code-aspx[Main](master-detail-filtering-across-two-pages-cs/samples/sample2.aspx)]

Dedicare alcuni minuti per visualizzare il `SupplierListMaster.aspx` pagina tramite un browser. Come illustrato nella figura 7, la pagina sono elencati tutti i fornitori contenente un collegamento di visualizzazione dei prodotti. Facendo clic su Visualizza prodotti collegamento consentirà di `ProductsForSupplierDetails.aspx`, passando lungo del fornitore `SupplierID` nella stringa di query.


[![Ogni riga fornitore contiene un collegamento di prodotti di visualizzazione](master-detail-filtering-across-two-pages-cs/_static/image18.png)](master-detail-filtering-across-two-pages-cs/_static/image17.png)

**Figura 7**: ogni riga fornitore contiene un collegamento di prodotti di visualizzazione ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-across-two-pages-cs/_static/image19.png))


## <a name="step-3-listing-the-suppliers-products-inproductsforsupplierdetailsaspx"></a>Passaggio 3: Elenco di prodotti del fornitore`ProductsForSupplierDetails.aspx`

A questo punto il `SupplierListMaster.aspx` pagina sta inviando agli utenti di `ProductsForSupplierDetails.aspx`, passando il fornitore selezionato `SupplierID` nella stringa di query. Passaggio finale dell'esercitazione è per visualizzare i prodotti in un controllo GridView in `ProductsForSupplierDetails.aspx` cui `SupplierID` è uguale al `SupplierID` passato tramite la stringa di query. Per eseguire l'avvio tramite l'aggiunta di un controllo GridView per il `ProductsForSupplierDetails.aspx` pagina utilizzando un nuovo controllo ObjectDataSource denominato `ProductsBySupplierDataSource` che richiama la `GetProductsBySupplierID(supplierID)` metodo la `ProductsBLL` classe.


[![Aggiungere un nuovo oggetto ObjectDataSource denominato ProductsBySupplierDataSource](master-detail-filtering-across-two-pages-cs/_static/image21.png)](master-detail-filtering-across-two-pages-cs/_static/image20.png)

**Figura 8**: aggiungere un nuovo denominato ObjectDataSource `ProductsBySupplierDataSource` ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-across-two-pages-cs/_static/image22.png))


[![Selezionare la classe ProductsBLL](master-detail-filtering-across-two-pages-cs/_static/image24.png)](master-detail-filtering-across-two-pages-cs/_static/image23.png)

**Figura 9**: selezionare il `ProductsBLL` classe ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-across-two-pages-cs/_static/image25.png))


[![Avere ObjectDataSource richiamare il metodo GetProductsBySupplierID(supplierID)](master-detail-filtering-across-two-pages-cs/_static/image27.png)](master-detail-filtering-across-two-pages-cs/_static/image26.png)

**Figura 10**: il ObjectDataSource richiamare il `GetProductsBySupplierID(supplierID)` metodo ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-across-two-pages-cs/_static/image28.png))


Il passaggio finale della procedura guidata Configura origine dati viene chiesto di specificare per l'origine di fornire il `GetProductsBySupplierID(supplierID)` del metodo *`supplierID`* parametro. Per utilizzare il valore di stringa di query, impostare l'origine del parametro QueryString e immettere il nome del valore stringa di query da utilizzare nella casella di testo QueryStringField (`SupplierID`).


[![Popolare il valore del parametro dal valore Querystring SupplierID supplierID](master-detail-filtering-across-two-pages-cs/_static/image30.png)](master-detail-filtering-across-two-pages-cs/_static/image29.png)

**Figura 11**: popolare il *`supplierID`* valore del parametro dal `SupplierID` valore Querystring ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-across-two-pages-cs/_static/image31.png))


Questo è tutto qui! Figura 12 illustra il `ProductsForSupplierDetails.aspx` pagina quando visitato facendo clic sul collegamento Tokyo Traders da `SupplierListMaster.aspx`.


[![Prodotti forniti dagli operatori Tokyo vengono visualizzati](master-detail-filtering-across-two-pages-cs/_static/image33.png)](master-detail-filtering-across-two-pages-cs/_static/image32.png)

**Figura 12**: il prodotti forniti dagli operatori Tokyo vengono visualizzati ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-across-two-pages-cs/_static/image34.png))


## <a name="displaying-supplier-information-inproductsforsupplierdetailsaspx"></a>Visualizzazione di informazioni del fornitore in`ProductsForSupplierDetails.aspx`

Come mostrato nella figura 12, il `ProductsForSupplierDetails.aspx` pagina elenca semplicemente i prodotti offerti dal `SupplierID` specificato nella stringa di query. Qualcuno ha inviato direttamente a questa pagina, tuttavia, potrebbe non sapere che figura 12 sono visualizzati prodotti Tokyo Traders. Per risolvere questo problema è possibile visualizzare informazioni relative al fornitore in questa pagina.

Per iniziare, aggiungere un controllo FormView sopra i prodotti GridView. Creare un nuovo controllo ObjectDataSource denominato `SuppliersDataSource` che richiama la `SuppliersBLL` della classe `GetSupplierBySupplierID(supplierID)` metodo.


[![Selezionare la classe SuppliersBLL](master-detail-filtering-across-two-pages-cs/_static/image36.png)](master-detail-filtering-across-two-pages-cs/_static/image35.png)

**Figura 13**: selezionare il `SuppliersBLL` classe ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-across-two-pages-cs/_static/image37.png))


[![Avere ObjectDataSource richiamare il metodo GetSupplierBySupplierID(supplierID)](master-detail-filtering-across-two-pages-cs/_static/image39.png)](master-detail-filtering-across-two-pages-cs/_static/image38.png)

**Figura 14**: il ObjectDataSource richiamare il `GetSupplierBySupplierID(supplierID)` metodo ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-across-two-pages-cs/_static/image40.png))


Come con la `ProductsBySupplierDataSource`, hanno il *`supplierID`* parametro assegnato il valore della `SupplierID` valore querystring.


[![Popolare il valore del parametro dal valore Querystring SupplierID supplierID](master-detail-filtering-across-two-pages-cs/_static/image42.png)](master-detail-filtering-across-two-pages-cs/_static/image41.png)

**Figura 15**: popolare il *`supplierID`* valore del parametro dal `SupplierID` valore Querystring ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-across-two-pages-cs/_static/image43.png))


Quando si associa il controllo FormView a ObjectDataSource nella visualizzazione progettazione, Visual Studio crea automaticamente il controllo FormView `ItemTemplate`, `InsertItemTemplate`, e `EditItemTemplate` con controlli Label e TextBox Web per ognuno dei campi di dati restituiti dal ObjectDataSource. Poiché si desidera semplicemente visualizzare fornitore informazioni contattarmi rimuovere il `InsertItemTemplate` e `EditItemTemplate`. Successivamente, Modifica ItemTemplate in modo che venga visualizzato il nome della società del fornitore in un `<h3>` elemento e l'indirizzo, città, paese e il numero di telefono sotto il nome della società. In alternativa, è possibile impostare manualmente il controllo FormView `DataSourceID` e creare il `ItemTemplate` markup, come è stato fatto nel "[la visualizzazione di dati con ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md)" esercitazione.

Dopo queste modifiche al markup dichiarativo del controllo FormView dovrebbe essere simile al seguente:


[!code-aspx[Main](master-detail-filtering-across-two-pages-cs/samples/sample3.aspx)]

La figura 16 mostra una cattura di schermata di `ProductsForSupplierDetails.aspx` dopo le informazioni di fornitore in precedenza sono stato incluse nella pagina.


[![L'elenco dei prodotti include un riepilogo sul fornitore](master-detail-filtering-across-two-pages-cs/_static/image45.png)](master-detail-filtering-across-two-pages-cs/_static/image44.png)

**Figura 16**: l'elenco dei prodotti include un riepilogo sul fornitore ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-across-two-pages-cs/_static/image46.png))


## <a name="applying-the-final-touches-for-theproductsforsupplierdetailsaspxui"></a>L'applicazione finale tocca per il`ProductsForSupplierDetails.aspx`dell'interfaccia utente

Per migliorare l'utente esperienza per questo report sono sono un paio di aggiunte si deve effettuare il `ProductsForSupplierDetails.aspx` pagina. Attualmente l'unico modo un utente può passare dal `ProductsForSupplierDetails.aspx` pagina tornare all'elenco di fornitori è fare clic sul pulsante Indietro del browser. Aggiungere un controllo collegamento ipertestuale per la `ProductsForSupplierDetails.aspx` pagina che consente di tornare al `SupplierListMaster.aspx`, fornendo un altro modo all'utente di tornare all'elenco principale.


[![Aggiungere un controllo collegamento ipertestuale per riportare l'utente in SupplierListMaster.aspx](master-detail-filtering-across-two-pages-cs/_static/image48.png)](master-detail-filtering-across-two-pages-cs/_static/image47.png)

**Figura 17**: aggiungere un controllo collegamento ipertestuale per riportare l'utente a `SupplierListMaster.aspx` ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-across-two-pages-cs/_static/image49.png))


Se l'utente fa clic sul collegamento Visualizza i prodotti per un fornitore che non dispone di tutti i prodotti di `ProductsBySupplierDataSource` ObjectDataSource in `ProductsForSupplierDetails.aspx` non verrà restituito alcun risultato. GridView associato a ObjectDataSource non eseguire il rendering di markup risultante in un'area vuota della pagina nel browser dell'utente. Per comunicare in modo più chiaro per l'utente che non sono presenti prodotti associati al fornitore selezionato è possibile impostare il controllo GridView `EmptyDataText` proprietà al messaggio di cui si desidera venga visualizzati quando si verifica una situazione. Impostata questa proprietà su "Non sono presenti prodotti forniti da questo fornitore"

Per impostazione predefinita, tutti i fornitori nel database Employees forniscono almeno un prodotto. Per questa esercitazione, tuttavia, è stata modificata manualmente il `Products` tabella in modo che il fornitore Escargots informazioni non è più associato a tutti i prodotti. Figura 18 Mostra la pagina dei dettagli per informazioni Escargots dopo aver eseguita questa modifica.


[![Gli utenti vengono informati che il fornitore non fornisce tutti i prodotti](master-detail-filtering-across-two-pages-cs/_static/image51.png)](master-detail-filtering-across-two-pages-cs/_static/image50.png)

**Figura 18**: gli utenti vengono informati che il fornitore non fornisce tutti i prodotti ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-across-two-pages-cs/_static/image52.png))


## <a name="summary"></a>Riepilogo

Mentre master-details report è possibile visualizzare i record master e di dettaglio in una singola pagina, in molti siti Web sono suddivisi in due pagine web. In questa esercitazione è stato illustrato come implementare tale una relazione master-Details con i fornitori elencati in un controllo GridView nella pagina web "master" e i prodotti associati alla pagina "Dettagli". Ogni riga del fornitore nella pagina master web contiene un collegamento alla pagina dei dettagli che passati lungo la riga `SupplierID` valore. Tali collegamenti specifiche della riga possono essere aggiunti facilmente mediante HyperLinkField del controllo GridView.

Nella pagina dei dettagli, il recupero di tali prodotti per il fornitore specificato è stato eseguito richiamando il `ProductsBLL` della classe `GetProductsBySupplierID(supplierID)` metodo. Il *`supplierID`* valore del parametro è stato specificato in modo dichiarativo utilizzando la stringa di query come origine del parametro. È stato anche illustrato come visualizzare i dettagli del fornitore nella pagina dettagli mediante un controllo FormView.

Il nostro [esercitazione successiva](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) è quello finale per i report master-details. Verrà esaminato come visualizzare un elenco di prodotti in un controllo GridView in cui ogni riga ha un pulsante Seleziona. Facendo clic sul pulsante Seleziona, verranno visualizzati i dettagli del prodotto in un controllo DetailsView nella stessa pagina.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore per questa esercitazione Hilton Giesenow. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](master-detail-filtering-with-two-dropdownlists-cs.md)
> [Successivo](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)
