---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-acess-two-pages-datalist-vb
title: Master-Details filtri tra due pagine (VB) | Documenti Microsoft
author: rick-anderson
description: "In questa esercitazione viene illustrato come separare un report master/dettaglio tra due pagine. Nella pagina 'master' è utilizzato un controllo Repeater per eseguire il rendering di un elenco di categ..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2010
ms.topic: article
ms.assetid: f1a1be2c-6fd9-4a09-916e-aa1b98d5cf17
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-acess-two-pages-datalist-vb
msc.type: authoredcontent
ms.openlocfilehash: 3f43fa998b81800cb1a2b7796ebb3922fc1caeb8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="masterdetail-filtering-across-two-pages-vb"></a>Master-Details filtri tra due pagine (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare App di esempio](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_34_VB.exe) o [Scarica il PDF](master-detail-filtering-acess-two-pages-datalist-vb/_static/datatutorial34vb1.pdf)

> In questa esercitazione viene illustrato come separare un report master/dettaglio tra due pagine. Nella pagina "master" utilizziamo un controllo Repeater per eseguire il rendering di un elenco di categorie che, quando è selezionato, richiederà all'utente alla pagina "Dettagli" in un controllo DataList due colonne Mostra i prodotti appartenenti alla categoria selezionata.


## <a name="introduction"></a>Introduzione

Nel [Master-Details filtro tra due pagine](../masterdetail/master-detail-filtering-across-two-pages-vb.md) esercitazione, viene esaminato questo modello usando un controllo GridView per visualizzare tutti i fornitori nel sistema. Questo controllo GridView è incluso un HyperLinkField, di cui eseguire il rendering come un collegamento a un'altra pagina, passando il `SupplierID` nella stringa di query. La seconda pagina utilizzato un GridView per elencare i prodotti specificati dal fornitore selezionato.

Tali report due pagine master/dettaglio può essere eseguita utilizzando i controlli DataList e Repeater. L'unica differenza è che DataList né Ripetitore fornisce supporto per il controllo HyperLinkField. In alternativa, è necessario aggiungere un controllo Web di collegamento ipertestuale o un elemento ancoraggio HTML (`<a>`) all'interno del controllo `ItemTemplate`. Il collegamento ipertestuale `NavigateUrl` proprietà o l'ancoraggio `href` attributo può quindi essere personalizzato utilizzando approcci dichiarativi o a livello di codice.

In questa esercitazione verrà illustrato un esempio in cui sono elencate le categorie in un elenco puntato in un'unica pagina utilizzando un controllo Repeater. Ogni elemento nell'elenco include il nome e descrizione, la categoria con il nome della categoria visualizzato come collegamento a un'altra pagina. Fare clic su questo collegamento verrà whisk all'utente la seconda pagina, in cui un controllo DataList visualizzerà i prodotti appartenenti alla categoria selezionata.

## <a name="step-1-displaying-the-categories-in-a-bulleted-list"></a>Passaggio 1: Visualizzare le categorie in un elenco puntato

Il primo passaggio nella creazione di qualsiasi report master-Details viene avviata tramite la visualizzazione dei record "master". Pertanto, la prima attività è per visualizzare le categorie nella pagina "master". Aprire il `CategoryListMaster.aspx` nella pagina di `DataListRepeaterFiltering` cartella, aggiungere un controllo Repeater e, dallo smart tag, scegliere di aggiungere un nuovo oggetto ObjectDataSource. Configurare il nuovo oggetto ObjectDataSource in modo che accede ai dati dal `CategoriesBLL` della classe `GetCategories` (metodo) (vedere la figura 1).


[![Configurare per l'utilizzo GetCategories metodo della classe CategoriesBLL ObjectDataSource](master-detail-filtering-acess-two-pages-datalist-vb/_static/image2.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image1.png)

**Figura 1**: configurare ObjectDataSource per utilizzare il `CategoriesBLL` della classe `GetCategories` metodo ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-acess-two-pages-datalist-vb/_static/image3.png))


Successivamente, definire i modelli del ripetitore in modo che venga visualizzato ogni nome di categoria e una descrizione come un elemento in un elenco puntato. Di seguito non ancora preoccuparsi con ogni categoria di collegamento alla pagina dei dettagli. Di seguito viene illustrato il markup dichiarativo per il controllo Repeater e ObjectDataSource:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample1.aspx)]

Con questo markup completo, richiedere qualche istante per visualizzare l'avanzamento attraverso un browser. Come illustrato nella figura 2, Ripetitore esegue il rendering come un elenco puntato contenente il nome e la descrizione di ogni categoria.


[![Ogni categoria viene visualizzata come voce di elenco puntato](master-detail-filtering-acess-two-pages-datalist-vb/_static/image5.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image4.png)

**Figura 2**: ogni categoria viene visualizzata come voce di elenco puntato ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-acess-two-pages-datalist-vb/_static/image6.png))


## <a name="step-2-turning-the-category-name-into-a-link-to-the-details-page"></a>Passaggio 2: Trasformare il nome della categoria in un collegamento alla pagina dei dettagli

Per consentire all'utente di visualizzare le informazioni "Dettagli" per una determinata categoria, è necessario aggiungere un collegamento a ogni elenco puntato di elemento che, quando è selezionato, richiederà all'utente la seconda pagina (`ProductsForCategoryDetails.aspx`). Questa seconda pagina visualizzerà i prodotti per la categoria selezionata utilizzando un controllo DataList. Per determinare la categoria è stato fatto clic con il collegamento, è necessario passare la categoria selezionata `CategoryID` alla seconda pagina tramite un meccanismo. Il modo più semplice, più semplice per trasferire dati scalare da una pagina è tramite la stringa di query, ovvero l'opzione che verrà utilizzata in questa esercitazione. In particolare, il `ProductsForCategoryDetails.aspx` pagina previsti selezionato  *`categoryID`*  valore deve essere passato tramite un campo stringa di query denominato `CategoryID`. Ad esempio, per visualizzare i prodotti per categoria delle bevande, che presenta un `CategoryID` pari a 1, un utente potrebbe visitare `ProductsForCategoryDetails.aspx?CategoryID=1`.

Per creare un collegamento ipertestuale per ogni elemento di elenco puntato nel Repeater è necessario aggiungere un controllo Web di collegamento ipertestuale o un elemento ancoraggio HTML (`<a>`) per il `ItemTemplate`. In scenari in cui il collegamento ipertestuale è visualizzato lo stesso per ogni riga, saranno sufficiente entrambi gli approcci. Per controlli Repeater, preferisco utilizzando l'elemento di ancoraggio. Per utilizzare l'elemento di ancoraggio, aggiornare ItemTemplate del Ripetitore per:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample2.aspx)]

Si noti che il `CategoryID` possono essere inserite direttamente all'interno dell'elemento ancoraggio `href` attributo; tuttavia, per così essere determinate delimitare il `href` il valore dell'attributo con (apostrofi e virgolette nota) dopo il `Eval` (metodo) all'interno di `href` attributo delimita la relativa stringa (`"CategoryID"`) con virgolette doppie. In alternativa, un controllo Web di collegamento ipertestuale può essere utilizzato invece:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample3.aspx)]

Si noti come la parte dell'URL statica: `ProductsForCategoryDetails.aspx?CategoryID` , viene aggiunto al risultato di `Eval("CategoryID")` direttamente nella sintassi dell'associazione dati mediante la concatenazione di stringhe.

Uno dei vantaggi dell'utilizzo del controllo collegamento ipertestuale è che si sia a livello di codice accessibile del ripetitore `ItemDataBound` gestore dell'evento, se necessario. Potrebbe ad esempio, si desidera visualizzare il nome della categoria come testo anziché come un collegamento per le categorie di prodotti non associati. Tale controllo è stato possibile eseguire a livello di codice nel `ItemDataBound` gestore di eventi; per le categorie senza prodotti associati, il collegamento ipertestuale del `NavigateUrl` proprietà può essere impostata su una stringa vuota, in tal modo risultante in quel determinato nome di categoria rendering come testo normale (anziché come collegamento). Fare riferimento al [formattazione di DataList e basato su dati Ripetitore](../displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-vb.md) esercitazione per ulteriori informazioni sulla formattazione di DataList e Repeater contenuto in base alla logica a livello di codice tramite la `ItemDataBound` gestore dell'evento.

Se si segue, è possibile utilizzare l'approccio con il controllo collegamento ipertestuale o elemento di ancoraggio della pagina. Indipendentemente dall'approccio, quando si visualizza la pagina tramite un browser di rendering di ogni nome di categoria deve essere eseguito come un collegamento a `ProductsForCategoryDetails.aspx`, passando l'applicabili `CategoryID` valore (vedere la figura 3).


[![I nomi delle categorie ora collegare ProductsForCategoryDetails.aspx](master-detail-filtering-acess-two-pages-datalist-vb/_static/image8.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image7.png)

**Figura 3**: la categoria nomi ora collegare `ProductsForCategoryDetails.aspx` ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-acess-two-pages-datalist-vb/_static/image9.png))


## <a name="step-3-listing-the-products-that-belong-to-the-selected-category"></a>Passaggio 3: Elenco dei prodotti che appartengono alla categoria selezionata

Con il `CategoryListMaster.aspx` pagina completa, si è pronti per passare alle pagina "Dettagli", implementazione `ProductsForCategoryDetails.aspx`. Aprire questa pagina, trascinare un controllo DataList dalla casella degli strumenti nella finestra di progettazione e impostare il relativo `ID` proprietà `ProductsInCategory`. Successivamente, smart tag del controllo DataList scegliere di aggiungere un nuovo oggetto ObjectDataSource alla pagina, denominarla `ProductsInCategoryDataSource`. Configurarlo in modo che chiami il `ProductsBLL` della classe `GetProductsByCategoryID(categoryID)` (metodo); impostare l'elenco a discesa sono elencati nelle schede delle INSERT, UPDATE e DELETE su (nessuno).


[![Configurare ObjectDataSource per l'utilizzo GetProductsByCategoryID(categoryID) (metodo della classe ProductsBLL)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image11.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image10.png)

**Figura 4**: configurare ObjectDataSource per utilizzare il `ProductsBLL` della classe `GetProductsByCategoryID(categoryID)` metodo ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-acess-two-pages-datalist-vb/_static/image12.png))


Poiché il `GetProductsByCategoryID(categoryID)` metodo accetta un parametro di input (*`categoryID`*), la procedura guidata Scegli origine dati offre la possibilità di specificare l'origine del parametro. Impostare l'origine del parametro QueryString utilizzando il QueryStringField `CategoryID`.


[![Utilizzare il parametro di campo Querystring CategoryID come origine del parametro](master-detail-filtering-acess-two-pages-datalist-vb/_static/image14.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image13.png)

**Figura 5**: utilizzare Querystring Field `CategoryID` come origine del parametro ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-acess-two-pages-datalist-vb/_static/image15.png))


Come abbiamo visto nelle esercitazioni precedenti, dopo aver completato la procedura guidata Scegli origine dati, Visual Studio crea automaticamente un `ItemTemplate` per DataList che elenca ogni valore e il nome del campo dati. Sostituire tale modello con uno che elenca solo nome del prodotto, fornitore e prezzo. Inoltre, impostare il controllo DataList `RepeatColumns` proprietà su 2. Dopo aver apportato queste modifiche le DataList ObjectDataSource markup dichiarativo dovrebbe essere simile al seguente:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample4.aspx)]

Per visualizzare questa pagina in azione, iniziare dal `CategoryListMaster.aspx` pagina, fare clic su un collegamento nell'elenco puntato di categorie. In questo modo verrà visualizzata `ProductsForCategoryDetails.aspx`, passando lungo il `CategoryID` tramite la stringa di query. Il `ProductsInCategoryDataSource` ObjectDataSource in `ProductsForCategoryDetails.aspx` verrà quindi recupera solo i prodotti per la categoria specificata e visualizzarle all'interno di DataList, che esegue il rendering di due prodotti per ogni riga. Figura 6 mostra una schermata di `ProductsForCategoryDetails.aspx` quando si visualizzano le bibite.


[![Vengono visualizzate le bibite, due per ogni riga](master-detail-filtering-acess-two-pages-datalist-vb/_static/image17.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image16.png)

**Figura 6**: vengono visualizzati il bevande, due per ogni riga ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-acess-two-pages-datalist-vb/_static/image18.png))


## <a name="step-4-displaying-category-information-on-productsforcategorydetailsaspx"></a>Passaggio 4: Visualizzazione di informazioni di categoria su ProductsForCategoryDetails.aspx

Quando un utente fa clic su una categoria in `CategoryListMaster.aspx`, vengono estratti `ProductsForCategoryDetails.aspx` e visualizzati i prodotti appartenenti alla categoria selezionata. Tuttavia, in `ProductsForCategoryDetails.aspx` non esistono segnali visivi per la categoria è stata selezionata. Un utente che deve fare clic su bibite ma condimenti accidentalmente si fa clic, non ha modo di sapere loro errore dopo aver raggiunto `ProductsForCategoryDetails.aspx`. Per risolvere questo problema, è possibile visualizzare informazioni sulla categoria selezionata, il nome e descrizione, nella parte superiore del `ProductsForCategoryDetails.aspx` pagina.

A tale scopo, aggiungere un controllo FormView sopra il controllo Repeater `ProductsForCategoryDetails.aspx`. Successivamente, aggiungere un nuovo oggetto ObjectDataSource alla pagina smart tag del controllo FormView denominato `CategoryDataSource` e configurarlo per utilizzare il `CategoriesBLL` della classe `GetCategoryByCategoryID(categoryID)` metodo.


[![Accedere alle informazioni sulla categoria di tramite il CategoriesBLL metodo della classe GetCategoryByCategoryID(categoryID)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image20.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image19.png)

**Figura 7**: accedere alle informazioni sulla categoria di tramite il `CategoriesBLL` della classe `GetCategoryByCategoryID(categoryID)` metodo ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-acess-two-pages-datalist-vb/_static/image21.png))


Come con la `ProductsInCategoryDataSource` ObjectDataSource aggiunto nel passaggio 3, il `CategoryDataSource`della configurazione guidata origine dati richiede a un'origine per il `GetCategoryByCategoryID(categoryID)` parametro di input del metodo. Utilizzare le stesse identiche impostazioni come prima, impostando l'origine di parametro di stringa di query e il valore QueryStringField da `CategoryID` (vedere la figura 5).

Dopo aver completato la procedura guidata, Visual Studio crea automaticamente un `ItemTemplate`, `EditItemTemplate`, e `InsertItemTemplate` per FormView. Poiché offriamo un'interfaccia di sola lettura, è possibile rimuovere il `EditItemTemplate` e `InsertItemTemplate`. Inoltre, è possibile personalizzare il controllo FormView `ItemTemplate`. Dopo la rimozione di modelli superflui e personalizzazione di ItemTemplate, markup dichiarativo del controllo FormView e ObjectDataSource dovrebbe essere simile al seguente:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample5.aspx)]

Figura 8 mostra una schermata durante la visualizzazione di questa pagina tramite un browser.

> [!NOTE]
> Oltre a FormView, ho aggiunto anche un controllo HyperLink sopra FormView che richiederà all'utente l'elenco delle categorie (`CategoryListMaster.aspx`). È possibile effettuare il collegamento in un' posizione o per ometterlo.


[![Informazioni sulla categoria è ora visualizzata nella parte superiore della pagina](master-detail-filtering-acess-two-pages-datalist-vb/_static/image23.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image22.png)

**Figura 8**: informazioni sulla categoria è ora visualizzata nella parte superiore della pagina ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-acess-two-pages-datalist-vb/_static/image24.png))


## <a name="step-5-displaying-a-message-if-no-products-belong-to-the-selected-category"></a>Passaggio 5: Visualizzazione di un messaggio se prodotti non appartengono alla categoria selezionata

Il `CategoryListMaster.aspx` pagina sono elencate tutte le categorie nel sistema, indipendentemente se sono presenti prodotti associati. Se un utente fa clic su una categoria con nessun prodotti associati, in DataList `ProductsForCategoryDetails.aspx` non vengono visualizzati, come origine dati non avrà alcun elemento. Come abbiamo visto nelle esercitazioni precedenti, il controllo GridView fornisce un `EmptyDataText` proprietà che può essere utilizzata per specificare un messaggio di testo da visualizzare se non sono presenti record nell'origine dati. Purtroppo, DataList né Ripetitore dispone di tale proprietà.

Per visualizzare un messaggio che informa l'utente che non sono presenti prodotti corrispondenti per la categoria selezionata, è necessario aggiungere un'etichetta di controllo alla pagina di cui `Text` proprietà viene assegnato il messaggio da visualizzare nel caso in cui non sono presenti prodotti corrispondenti. È quindi necessario impostare a livello di codice relativo `Visible` proprietà dipende o meno DataList contiene elementi.

A tale scopo, per iniziare, aggiungere un'etichetta di sotto di DataList. Impostare il relativo `ID` proprietà `NoProductsMessage` e il relativo `Text` proprietà su "Non sono presenti prodotti per la categoria selezionata..." Successivamente, è necessario impostare a livello di codice questa etichetta `Visible` dipende o meno tutti i dati è stati associati a proprietà di `ProductsInCategory` DataList. Questa assegnazione deve essere effettuata dopo aver associati i dati di DataList. Per il controllo GridView, DetailsView e FormView, è possibile creare un gestore eventi per il controllo `DataBound` evento, viene generato dopo l'associazione dati è stata completata. Tuttavia, ha DataList né Repeater un `DataBound` eventi disponibili.

In questo esempio specifico è possibile assegnare l'etichetta `Visible` proprietà il `Page_Load` gestore dell'evento, poiché i dati verranno assegnati a DataList prima della pagina `Load` evento. Questo approccio non funzionerà nel caso generale, come i dati da ObjectDataSource sia associati a DataList nel ciclo di vita della pagina. Ad esempio, se i dati visualizzati si basano il valore in un altro controllo, ad esempio quando si visualizza un report master-Details mediante un DropDownList per contenere i record "master", i dati potrebbero non verrà riassociato al controllo Web dati fino a quando il `PreRender` fase nel ciclo di vita della pagina.

Una soluzione che funziona per tutti i casi consiste nell'assegnare il `Visible` proprietà `False` del controllo DataList `ItemDataBound` (o `ItemCreated`) gestore dell'evento quando un tipo di elemento di associazione `Item` o `AlternatingItem`. In questo caso si sa che vi sia almeno un dato elemento nell'origine dati e pertanto è possibile nascondere il `NoProductsMessage` etichetta. Oltre a questo gestore eventi, è necessario anche un gestore eventi per il controllo DataList `DataBinding` evento, in cui si inizializza l'etichetta `Visible` proprietà `True`. Poiché il `DataBinding` evento viene generato prima di `ItemDataBound` del eventi, l'etichetta `Visible` verrà inizialmente impostata su `True`; se sono presenti elementi di dati, tuttavia, verrà impostata su `False`. Il codice seguente implementa la logica:

[!code-vb[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample6.vb)]

Tutte le categorie del database Northwind sono associate uno o più prodotti. Per testare questa funzionalità, ho regolato manualmente il database Northwind per questa esercitazione, la riassegnazione di tutti i prodotti associati alla categoria di prodotto (`CategoryID` = 7) per la categoria di frutti di mare (`CategoryID` = 8). Questo può essere eseguito da Esplora Server scegliendo Nuova Query e con i seguenti `UPDATE` istruzione:

[!code-sql[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample7.sql)]

Dopo aver aggiornato di conseguenza il database, ripristinare il `CategoryListMaster.aspx` pagina e fare clic sul collegamento producono. Poiché non sono più presenti tutti i prodotti appartenenti alla categoria di prodotto, dovrebbe essere il "Non sono presenti prodotti per la categoria selezionata..." messaggio, come illustrato nella figura 9.


[![Se sono presenti n prodotti appartenenti alla categoria selezionata, viene visualizzato un messaggio](master-detail-filtering-acess-two-pages-datalist-vb/_static/image26.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image25.png)

**Figura 9**: viene visualizzato un messaggio se sono presenti n prodotti appartenenti alla categoria selezionata ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-acess-two-pages-datalist-vb/_static/image27.png))


## <a name="summary"></a>Riepilogo

Mentre master-details report è possibile visualizzare i record master e di dettaglio in una singola pagina, in molti siti Web sono suddivisi in due pagine web. In questa esercitazione è stato illustrato come implementare tale una relazione master-Details con categorie elencate in un elenco puntato utilizzando un ripetitore nella pagina web "master" e i prodotti associati alla pagina "Dettagli". Ogni elemento di elenco nella pagina master web contenuto un collegamento alla pagina dei dettagli che passati lungo la riga `CategoryID` valore.

Nella pagina dei dettagli del recupero di tali prodotti per il fornitore specificato è stato eseguito tramite il `ProductsBLL` della classe `GetProductsByCategoryID(categoryID)` metodo. Il  *`categoryID`*  valore del parametro è stato specificato in modo dichiarativo utilizzando la `CategoryID` valore querystring come origine del parametro. È stato anche illustrato come visualizzare i dettagli della categoria nella pagina dettagli mediante un controllo FormView e come se non fossero prodotti appartenenti alla categoria selezionata viene visualizzato un messaggio.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali...

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Lead revisori per questa esercitazione sono stati Zack Jones e Liz Shulok. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Precedente](master-detail-filtering-with-a-dropdownlist-datalist-vb.md)
[Successivo](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md)
