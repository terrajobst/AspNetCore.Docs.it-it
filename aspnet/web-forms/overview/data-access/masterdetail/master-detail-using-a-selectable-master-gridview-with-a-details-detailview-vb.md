---
uid: web-forms/overview/data-access/masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb
title: Master/dettaglio con Master GridView selezionabile e un controllo DetailView per i dettagli (VB) | Microsoft Docs
author: rick-anderson
description: Questa esercitazione sarà necessario un controllo GridView cui righe includono il nome e il prezzo di ogni prodotto insieme a un pulsante di selezione. Facendo clic sul pulsante Seleziona per un particu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 1d1a7c93-971d-4690-9c5e-dac0e5014a09
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb
msc.type: authoredcontent
ms.openlocfilehash: a97323700e20aed12ee29674952f1ffe9144133c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366041"
---
<a name="masterdetail-using-a-selectable-master-gridview-with-a-details-detailview-vb"></a>Master/dettaglio con Master GridView selezionabile e un controllo DetailView per i dettagli (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'App di esempio](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_10_VB.exe) o [Scarica il PDF](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/datatutorial10vb1.pdf)

> Questa esercitazione sarà necessario un controllo GridView cui righe includono il nome e il prezzo di ogni prodotto insieme a un pulsante di selezione. Facendo clic sul pulsante Seleziona per un determinato prodotto comporterà i dettagli completi da visualizzare nel controllo DetailsView nella stessa pagina.


## <a name="introduction"></a>Introduzione

Nel [esercitazione precedente](master-detail-filtering-across-two-pages-vb.md) è stato illustrato come creare un report master-Details mediante due pagine web: una pagina web "master", da cui è visualizzato l'elenco di fornitori e una pagina web "Dettagli" elencati questi prodotti forniti dall'oggetto selezionato fornitore. È preferibile ridurre questo formato di report di due pagine in un'unica pagina. Questa esercitazione sarà necessario un controllo GridView cui righe includono il nome e il prezzo di ogni prodotto insieme a un pulsante di selezione. Facendo clic sul pulsante Seleziona per un determinato prodotto comporterà i dettagli completi da visualizzare nel controllo DetailsView nella stessa pagina.


[![Fare clic sul pulsante Seleziona Visualizza i dettagli del prodotto](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image2.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image1.png)

**Figura 1**: fare clic sul pulsante Seleziona Visualizza i dettagli del prodotto ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image3.png))


## <a name="step-1-creating-a-selectable-gridview"></a>Passaggio 1: Creazione GridView selezionabile

Tenere presente che nei due pagine master/dettagli del report che ogni record master incluso un collegamento ipertestuale che, quando si fa clic, inviati l'utente alla pagina dei dettagli del passaggio della riga selezionata `SupplierID` valore nella stringa di query. Tale collegamento è stato aggiunto a ogni riga GridView mediante un HyperLinkField. Affinché il rapporto master/dettagli pagina singola, è necessario per ogni controllo GridView di riga che, quando si fa clic, viene visualizzato un pulsante i dettagli. Il controllo GridView può essere configurato per includere un pulsante Seleziona per ogni riga che determina un postback e tale riga viene contrassegnata come di GridView [SelectedRow](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedrow.aspx).

Iniziare aggiungendo un controllo GridView per il `DetailsBySelecting.aspx` nella pagina la `Filtering` cartella, l'impostazione relativa `ID` proprietà `ProductsGrid`. Successivamente, aggiungere un nuovo oggetto ObjectDataSource denominato `AllProductsDataSource` che richiama la `ProductsBLL` della classe `GetProducts()` (metodo).


[![Creare un oggetto ObjectDataSource denominato AllProductsDataSource](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image5.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image4.png)

**Figura 2**: creare un oggetto ObjectDataSource denominato `AllProductsDataSource` ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image6.png))


[![Usare la classe ProductsBLL](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image8.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image7.png)

**Figura 3**: usare il `ProductsBLL` classe ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image9.png))


[![Configurare ObjectDataSource per richiamare il metodo GetProducts()](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image11.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image10.png)

**Figura 4**: configurare ObjectDataSource per richiamare il `GetProducts()` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image12.png))


Modificare i campi di GridView rimuovendo tutto tranne le `ProductName` e `UnitPrice` BoundField. Inoltre, è possibile personalizzare questi BoundField in base alle esigenze, ad esempio la formattazione il `UnitPrice` BoundField come valuta e modificando il `HeaderText` le proprietà dei BoundField. Questi passaggi possono essere eseguiti in formato grafico facendo clic sul collegamento Modifica colonne dallo smart tag del controllo GridView, o configurare manualmente la sintassi dichiarativa.


[![Rimuovi tutto tranne il ProductName e un BoundField UnitPrice](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image14.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image13.png)

**Figura 5**: rimuovere tutto tranne la `ProductName` e `UnitPrice` BoundField ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image15.png))


Il markup finale per il controllo GridView è:


[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample1.aspx)]

Successivamente, è necessario contrassegnare il controllo GridView come selezionabile, che consentirà di aggiungere un pulsante Seleziona per ciascuna riga. A tale scopo, è sufficiente selezionare la casella di controllo di selezione attiva nello smart tag del controllo GridView.


[![Rendere selezionabile righe del controllo GridView](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image17.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image16.png)

**Figura 6**: rendere selezionabile di righe del controllo GridView ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image18.png))


Selezionando l'opzione di abilitazione della selezione viene aggiunto un CommandField per il `ProductsGrid` GridView con relativo `ShowSelectButton` proprietà è impostata su True. Ciò comporta un pulsante Seleziona per ciascuna riga di GridView, come illustrato nella figura 6. Per impostazione predefinita, vengono visualizzati i pulsanti di selezione come LinkButton, ma è possibile usare i pulsanti o ImageButtons invece tramite la CommandField `ButtonType` proprietà.


[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample2.aspx)]

Quando si fa clic sul pulsante di selezione di una riga GridView un postback previsioni e del controllo GridView `SelectedRow` proprietà viene aggiornata. Oltre al `SelectedRow` fornisce il controllo GridView di proprietà, il [SelectedIndex](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindex%28VS.80%29.aspx), [SelectedValue](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedvalue%28VS.80%29.aspx), e [SelectedDataKey](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selecteddatakey%28VS.80%29.aspx) proprietà. Il `SelectedIndex` proprietà restituisce l'indice della riga selezionata, mentre la `SelectedValue` e `SelectedDataKey` restituiscono valori in base al GridView [DataKeyNames proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.datakeynames%28VS.80%29.aspx).

Il `DataKeyNames` proprietà viene utilizzata per associare uno o più campi dati valori o con ogni riga e viene comunemente usato per le informazioni di identificazione univoca dei dati sottostanti con ogni riga GridView degli attributi. Il `SelectedValue` proprietà restituisce il valore del primo `DataKeyNames` campo di dati per la riga selezionata mentre il `SelectedDataKey` proprietà restituisce la riga selezionata `DataKey` oggetto che contiene tutti i valori per i campi di chiave specificati dati per tale riga.

Il `DataKeyNames` proprietà viene impostata automaticamente per i campi di dati che identifica in modo univoco quando si associa un'origine dati a un GridView, DetailsView e FormView tramite la finestra di progettazione. Anche se questa proprietà è stata impostata per noi automaticamente nelle esercitazioni precedenti, gli esempi avrebbe avuto esito positivo senza il `DataKeyNames` proprietà specificata. Tuttavia, per il controllo GridView selezionabili in questa esercitazione, nonché per in cui si verrà esaminando inserimento, aggiornamento ed eliminazione, nelle esercitazioni successive il `DataKeyNames` deve essere impostata correttamente. Si consiglia di verificare che i GridView `DataKeyNames` è impostata su `ProductID`.

È possibile visualizzare lo stato di avanzamento fino a quel momento tramite un browser. Si noti che il controllo GridView sono elencati il nome e il prezzo per tutti i prodotti con un controllo LinkButton selezionare. Fare clic sul pulsante Select determina un postback. Nel passaggio 2 vedremo come disporre un controllo DetailsView possa rispondere il postback visualizzando i dettagli per il prodotto selezionato.


[![Ogni riga del prodotto contiene un controllo LinkButton selezionare](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image20.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image19.png)

**Figura 7**: ogni riga del prodotto contiene un controllo LinkButton selezionare ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image21.png))


## <a name="highlighting-the-selected-row"></a>L'evidenziazione della riga selezionata

Il `ProductsGrid` include anche un `SelectedRowStyle` proprietà che può essere usata per determinare lo stile di visualizzazione per la riga selezionata. Utilizzata in modo corretto, ciò può migliorare l'esperienza dell'utente mostrando più chiaramente quale riga di GridView attualmente selezionata. Per questa esercitazione, è possibile avere la riga selezionata evidenziata con uno sfondo giallo.

Come con le esercitazioni precedenti, si impegna a mantenere le impostazioni relative alla estetici definite come classi CSS. Pertanto, creare una nuova classe CSS `Styles.css` denominato `SelectedRowStyle`.


[!code-css[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample3.css)]

Da questa classe CSS da applicare il `SelectedRowStyle` proprietà di *tutti* GridView nella nostra serie di esercitazioni, modificare il `GridView.skin` interfaccia personalizzata nel `DataWebControls` tema per includere il `SelectedRowStyle` impostazioni come illustrato di seguito:


[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample4.aspx)]

Grazie a questa aggiunta, la riga selezionata di GridView a questo punto verrà evidenziata da un colore di sfondo giallo.


[![Personalizzare l'aspetto della riga selezionata tramite proprietà SelectedRowStyle di GridView](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image23.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image22.png)

**Figura 8**: personalizzare usando l'aspetto della riga selezionata di GridView `SelectedRowStyle` proprietà ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image24.png))


## <a name="step-2-displaying-the-selected-products-details-in-a-detailsview"></a>Passaggio 2: Visualizzazione dei dettagli del prodotto selezionato in un controllo DetailsView

Con la `ProductsGrid` completare GridView, tutto ciò che resta consiste nell'aggiungere un controllo DetailsView che visualizza le informazioni del prodotto specifico selezionato. Aggiungere un controllo DetailsView sopra il controllo GridView e creare un nuovo oggetto ObjectDataSource denominato `ProductDetailsDataSource`. Poiché si desidera che questa DetailsView per visualizzare determinate informazioni relative al prodotto selezionato, configurare il `ProductDetailsDataSource` usare il `ProductsBLL` della classe `GetProductByProductID(productID)` (metodo).


[![Richiamare metodo della classe GetProductByProductID(productID) ProductsBLL](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image26.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image25.png)

**Figura 9**: richiamare il `ProductsBLL` della classe `GetProductByProductID(productID)` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image27.png))


Disporre le *`productID`* valore del parametro ottenuto dalla proprietà del controllo GridView `SelectedValue` proprietà. Come accennato in precedenza, il controllo GridView `SelectedValue` proprietà restituisce il primo valore chiave dati per la riga selezionata. Pertanto, è fondamentale che il controllo GridView `DataKeyNames` è impostata su `ProductID`, in modo che la riga selezionata `ProductID` valore viene restituito da `SelectedValue`.


[![Impostare il parametro productID alla proprietà SelectedValue di GridView](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image29.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image28.png)

**Figura 10**: impostare il *`productID`* parametro per il controllo GridView `SelectedValue` proprietà ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image30.png))


Una volta il `productDetailsDataSource` ObjectDataSource è stato configurato correttamente e associato a DetailsView, questa esercitazione è stata completata. Quando la pagina viene prima visitata è selezionata alcuna riga, pertanto il GridView `SelectedValue` restituisce proprietà `Nothing`. Poiché non sono presenti prodotti con un `NULL` `ProductID` valore, viene restituito alcun record per il `GetProductByProductID(productID)` metodo, vale a dire che non viene visualizzato il controllo DetailsView (vedere la figura 11). Facendo clic sul pulsante di selezione di una riga GridView, un postback previsioni e DetailsView viene aggiornato. Si time del controllo GridView `SelectedValue` proprietà restituisce il `ProductID` della riga selezionata, il `GetProductByProductID(productID)` metodo restituisce un `ProductsDataTable` con informazioni su quel particolare prodotto e DetailsView Mostra questi dettagli (vedere la figura 12).


[![Quando viene visualizzata prima visita, solo il controllo GridView](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image32.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image31.png)

**Figura 11**: quando prima visita, viene visualizzato solo il controllo GridView ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image33.png))


[![Dopo aver selezionato una riga, vengono visualizzati i dettagli del prodotto](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image35.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image34.png)

**Figura 12**: dopo aver selezionato una riga, vengono visualizzati i dettagli del prodotto ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image36.png))


## <a name="summary"></a>Riepilogo

In questo e le tre esercitazioni precedenti abbiamo visto una serie di tecniche per la visualizzazione dei report master o di dettaglio. In questa esercitazione che sono stati esaminati con GridView selezionabile per ospitare i record master e un DetailsView per visualizzare i dettagli del record master selezionato nella stessa pagina. Nelle esercitazioni precedenti è stato descritto come visualizzare i report master-details mediante controlli DropDownList e sulla visualizzazione di record master in una pagina web e i record di dettaglio in un altro.

Questa esercitazione è terminata l'esame del report master o di dettaglio. Inizia con l'esercitazione successiva inizieremo l'esplorazione della formattazione personalizzata con il controllo GridView, DetailsView e FormView. Si vedrà come personalizzare l'aspetto di questi controlli in base ai dati associati ad essi, come riepilogare i dati in un piè di pagina del controllo GridView e come usare i modelli per ottenere un maggiore grado di controllare il layout.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Hilton Giesenow, revisore per questa esercitazione. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](master-detail-filtering-across-two-pages-vb.md)
