---
uid: web-forms/overview/data-access/masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs
title: Master-Details mediante Master GridView selezionabile con un DetailView dettagli (c#) | Documenti Microsoft
author: rick-anderson
description: In questa esercitazione sarà necessario un controllo GridView le cui righe includono il nome e il prezzo di ogni prodotto insieme a un pulsante Seleziona. Fare clic sul pulsante di seleziona per un particu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 0f982827-f8f9-420d-b36b-57b23f5aa519
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs
msc.type: authoredcontent
ms.openlocfilehash: 9d39786cb17449b93e6f728a0a3c920e1be089be
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30883027"
---
<a name="masterdetail-using-a-selectable-master-gridview-with-a-details-detailview-c"></a>Master-Details mediante Master GridView selezionabile con un DetailView dettagli (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare App di esempio](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_10_CS.exe) o [Scarica il PDF](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/datatutorial10cs1.pdf)

> In questa esercitazione sarà necessario un controllo GridView le cui righe includono il nome e il prezzo di ogni prodotto insieme a un pulsante Seleziona. Fare clic sul pulsante di seleziona per un determinato prodotto causerà i dettagli completi per essere visualizzati in un controllo DetailsView nella stessa pagina.


## <a name="introduction"></a>Introduzione

Nel [esercitazione precedente](master-detail-filtering-across-two-pages-cs.md) è stato illustrato come creare un report master-Details mediante due pagine web: una pagina web "master", da cui è visualizzato l'elenco di fornitori; e una pagina web "Dettagli" elencati questi prodotti forniti dall'oggetto selezionato fornitore. Questo formato di report di due pagine possa essere condensato in un'unica pagina. In questa esercitazione sarà necessario un controllo GridView le cui righe includono il nome e il prezzo di ogni prodotto insieme a un pulsante Seleziona. Fare clic sul pulsante di seleziona per un determinato prodotto causerà i dettagli completi per essere visualizzati in un controllo DetailsView nella stessa pagina.


[![Fare clic sul pulsante di seleziona vengono visualizzati i dettagli del prodotto](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image2.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image1.png)

**Figura 1**: fare clic sul pulsante di seleziona vengono visualizzati i dettagli del prodotto ([fare clic per visualizzare l'immagine ingrandita](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image3.png))


## <a name="step-1-creating-a-selectable-gridview"></a>Passaggio 1: Creazione di un oggetto selezionabile GridView

Tenere presente che nei due pagine master/dettagli report che ogni record master è incluso un collegamento ipertestuale che, quando si fa clic, inviato l'utente alla pagina dei dettagli della riga selezionata passando `SupplierID` valore nella stringa di query. Tale collegamento è stato aggiunto a ogni riga GridView mediante un HyperLinkField. Per il report master/dettagli singola pagina, è necessario un pulsante per ogni controllo GridView di riga che, quando si fa clic, consente di visualizzare i dettagli. Il controllo GridView può essere configurato per includere un pulsante Seleziona per ogni riga che causa un postback e tale riga viene contrassegnata come di GridView [SelectedRow](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedrow.aspx).

Per iniziare, aggiungere un controllo GridView per il `DetailsBySelecting.aspx` nella pagina di `Filtering` cartella, l'impostazione relativa `ID` proprietà `ProductsGrid`. Successivamente, aggiungere un nuovo oggetto ObjectDataSource denominato `AllProductsDataSource` che richiama la `ProductsBLL` della classe `GetProducts()` metodo.


[![Creare un ObjectDataSource denominato AllProductsDataSource](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image5.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image4.png)

**Figura 2**: creare un denominato ObjectDataSource `AllProductsDataSource` ([fare clic per visualizzare l'immagine ingrandita](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image6.png))


[![Utilizzare la classe ProductsBLL](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image8.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image7.png)

**Figura 3**: usare il `ProductsBLL` classe ([fare clic per visualizzare l'immagine ingrandita](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image9.png))


[![Configurare ObjectDataSource per richiamare il metodo GetProducts()](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image11.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image10.png)

**Figura 4**: configurare ObjectDataSource per richiamare il `GetProducts()` metodo ([fare clic per visualizzare l'immagine ingrandita](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image12.png))


Modificare i campi del controllo GridView rimozione tutto tranne il `ProductName` e `UnitPrice` BoundField. Inoltre, è possibile personalizzare questi BoundField in base alle esigenze, ad esempio la formattazione di `UnitPrice` BoundField come valuta e modifica il `HeaderText` le proprietà del BoundField. Questi passaggi possono essere effettuati in formato grafico facendo clic sul collegamento Modifica colonne smart tag del controllo GridView o configurare manualmente la sintassi dichiarativa.


[![Rimuovere tutto tranne il ProductName e UnitPrice BoundField](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image14.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image13.png)

**Figura 5**: rimuovere tutto tranne il `ProductName` e `UnitPrice` BoundField ([fare clic per visualizzare l'immagine ingrandita](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image15.png))


Il markup finale per il controllo GridView è:


[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/samples/sample1.aspx)]

Successivamente, è necessario contrassegnare il controllo GridView come selezionabile, che verrà aggiunto un pulsante Seleziona insieme a ogni riga. A tale scopo, è sufficiente selezionare la casella di controllo Abilita selezione smart tag del controllo GridView.


[![Rendere selezionabile righe del controllo GridView](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image17.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image16.png)

**Figura 6**: assicurarsi righe selezionabile di GridView ([fare clic per visualizzare l'immagine ingrandita](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image18.png))


L'opzione di attivazione di selezione il controllo consente di aggiungere un CommandField per il `ProductsGrid` GridView con relativo `ShowSelectButton` proprietà è impostata su True. Ciò comporta un pulsante Seleziona per ogni riga di GridView, come illustrato nella figura 6. Per impostazione predefinita, i pulsanti di selezione vengono eseguiti come LinkButton, ma è possibile utilizzare i pulsanti o ImageButtons invece tramite la CommandField `ButtonType` proprietà.


[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/samples/sample2.aspx)]

Quando si fa clic sul pulsante Seleziona di una riga controllo GridView viene utilizzata un postback e il controllo GridView `SelectedRow` proprietà viene aggiornata. Oltre al `SelectedRow` GridView proprietà, fornisce il [SelectedIndex](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindex%28VS.80%29.aspx), [SelectedValue](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedvalue%28VS.80%29.aspx), e [SelectedDataKey](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selecteddatakey%28VS.80%29.aspx) proprietà. Il `SelectedIndex` proprietà restituisce l'indice della riga selezionata, mentre il `SelectedValue` e `SelectedDataKey` restituiscono valori in base al momento di GridView [proprietà DataKeyNames](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.datakeynames%28VS.80%29.aspx).

Il `DataKeyNames` proprietà viene utilizzata per associare uno o più campi di dati i valori con ogni riga e viene utilizzato comunemente per le informazioni di identificazione in modo univoco dai dati sottostanti con ogni riga GridView dell'attributo. Il `SelectedValue` proprietà restituisce il valore del primo `DataKeyNames` campo dei dati per la riga selezionata mentre il `SelectedDataKey` proprietà restituisce la riga selezionata `DataKey` oggetto che contiene tutti i valori per i campi chiave di dati specificato per tale riga.

Il `DataKeyNames` proprietà viene impostata automaticamente per i campi di dati che identifica in modo univoco, quando si associa un'origine dati a GridView, DetailsView o FormView tramite la finestra di progettazione. Anche se questa proprietà è stata impostata per noi automaticamente nelle esercitazioni precedenti, gli esempi avrebbe avuto esito positivo senza il `DataKeyNames` proprietà specificata. Tuttavia, per GridView selezionabile in questa esercitazione, nonché per le esercitazioni successive in cui è possibile esaminare inserimento, aggiornamento ed eliminazione, il `DataKeyNames` deve essere impostata correttamente. È opportuno verificare che il GridView `DataKeyNames` è impostata su `ProductID`.

Consente di visualizzare l'avanzamento finora tramite un browser. Si noti che il controllo GridView Elenca il nome e il prezzo per tutti i prodotti insieme LinkButton selezionare. Fare clic sul pulsante Seleziona provoca un postback. Nel passaggio 2 vedremo come disporre di un controllo DetailsView rispondere a questo postback visualizzando i dettagli per il prodotto selezionato.


[![Ogni riga di Product contiene LinkButton Select](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image20.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image19.png)

**Figura 7**: ogni riga di Product contiene LinkButton selezionare ([fare clic per visualizzare l'immagine ingrandita](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image21.png))


### <a name="highlighting-the-selected-row"></a>Evidenziare la riga selezionata

Il `ProductsGrid` GridView ha un `SelectedRowStyle` proprietà che può essere utilizzata per determinare lo stile di visualizzazione per la riga selezionata. Utilizzata in modo corretto, ciò può migliorare l'esperienza dell'utente mostrando più chiaramente quale riga di GridView è attualmente selezionata. Per questa esercitazione, si dispone della riga selezionata evidenziata con uno sfondo giallo.

Come con le esercitazioni precedenti, si cercano di mantenere le impostazioni correlate estetici definite come classi CSS. Pertanto, creare una nuova classe CSS in `Styles.css` denominato `SelectedRowStyle`.


[!code-css[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/samples/sample3.css)]

Per applicare la classe CSS per il `SelectedRowStyle` proprietà di *tutti* GridView nella serie di esercitazioni, modificare il `GridView.skin` interfaccia personalizzata nel `DataWebControls` tema per includere il `SelectedRowStyle` impostazioni come indicato di seguito:


[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/samples/sample4.aspx)]

Con l'aggiunta, la riga selezionata di GridView viene ora evidenziata con un colore di sfondo giallo.


[![Personalizzare l'aspetto della riga selezionata utilizzando proprietà SelectedRowStyle di GridView](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image23.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image22.png)

**Figura 8**: personalizzare utilizzando la riga selezionata l'aspetto del controllo GridView `SelectedRowStyle` proprietà ([fare clic per visualizzare l'immagine ingrandita](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image24.png))


## <a name="step-2-displaying-the-selected-products-details-in-a-detailsview"></a>Passaggio 2: Visualizzazione dei dettagli del prodotto selezionato in un controllo DetailsView.

Con il `ProductsGrid` completare GridView, tutto ciò che è comunque necessario aggiungere un controllo DetailsView che visualizza informazioni sul prodotto specifico selezionato. Aggiungere un controllo DetailsView sopra GridView e creare un nuovo oggetto ObjectDataSource denominato `ProductDetailsDataSource`. Poiché si desidera che questo controllo DetailsView per visualizzare determinate informazioni relative al prodotto selezionato, configurare il `ProductDetailsDataSource` per utilizzare il `ProductsBLL` della classe `GetProductByProductID(productID)` metodo.


[![Richiamare il ProductsBLL metodo della classe GetProductByProductID(productID)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image26.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image25.png)

**Figura 9**: richiamare il `ProductsBLL` della classe `GetProductByProductID(productID)` metodo ([fare clic per visualizzare l'immagine ingrandita](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image27.png))


Dispone il *`productID`* valore del parametro ottenuto del controllo GridView `SelectedValue` proprietà. Come accennato in precedenza, il controllo GridView `SelectedValue` proprietà restituisce il primo della chiave dati valore per la riga selezionata. Pertanto, è fondamentale che il controllo GridView `DataKeyNames` è impostata su `ProductID`, in modo che la riga selezionata `ProductID` valore viene restituito da `SelectedValue`.


[![Impostare il parametro productID alla proprietà SelectedValue di GridView](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image29.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image28.png)

**Figura 10**: impostare il *`productID`* parametro per il controllo GridView `SelectedValue` proprietà ([fare clic per visualizzare l'immagine ingrandita](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image30.png))


Una volta il `productDetailsDataSource` ObjectDataSource è stato configurato correttamente e associato al controllo DetailsView., in questa esercitazione è stata completata. Quando viene innanzitutto visitata viene selezionata alcuna riga, pertanto il GridView `SelectedValue` restituisce proprietà `null`. Poiché non sono presenti prodotti con un `NULL` `ProductID` valore, viene restituito alcun record per il `GetProductByProductID(productID)` (metodo), vale a dire che il controllo DetailsView non è visualizzato (vedere Figura 11). Facendo clic sul pulsante Seleziona di una riga controllo GridView, viene utilizzata un postback e DetailsView viene aggiornato. In questo caso il GridView `SelectedValue` proprietà restituisce il `ProductID` della riga selezionata, il `GetProductByProductID(productID)` metodo restituisce un `ProductsDataTable` con informazioni sul prodotto specifico e di DetailsView Mostra questi dettagli (vedere Figura 12).


[![Quando viene visualizzata prima visitata, solo controllo GridView.](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image32.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image31.png)

**Figura 11**: quando prima visita, viene visualizzato solo il GridView ([fare clic per visualizzare l'immagine ingrandita](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image33.png))


[![Dopo aver selezionato una riga, vengono visualizzati i dettagli del prodotto](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image35.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image34.png)

**Figura 12**: dopo aver selezionato una riga, vengono visualizzati i dettagli del prodotto ([fare clic per visualizzare l'immagine ingrandita](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image36.png))


## <a name="summary"></a>Riepilogo

In questo esempio e i tre esercitazioni precedenti abbiamo visto un numero di tecniche per la visualizzazione master-details report. In questa esercitazione che viene esaminato utilizzando un GridView selezionabile per ospitare i record master e un controllo DetailsView visualizzare i dettagli relativi al record master selezionato nella stessa pagina. Nelle esercitazioni precedenti abbiamo esaminato la modalità di visualizzazione master/dettagli report utilizzando i controlli DropDownList e visualizzazione dei record master in una pagina web e i record di dettaglio in un altro.

Questa esercitazione si conclude l'esame del report master-details. A partire dal successivo tutorialwe innanzitutto l'esplorazione della formattazione personalizzata con GridView, DetailsView e FormView. Vedremo come personalizzare l'aspetto di questi controlli in base ai dati ad essi associati, come riepilogare i dati piè di pagina di GridView e come utilizzare i modelli per ottenere un maggiore livello di controllo del layout.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore per questa esercitazione Hilton Giesenow. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](master-detail-filtering-across-two-pages-cs.md)
> [Successivo](master-detail-filtering-with-a-dropdownlist-vb.md)
