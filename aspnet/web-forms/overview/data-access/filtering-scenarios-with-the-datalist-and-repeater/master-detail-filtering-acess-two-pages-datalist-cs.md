---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-acess-two-pages-datalist-cs
title: Master/dettagli filtro su due pagine (c#) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione verrà esaminata separare un report master o di dettaglio su due pagine. Nella pagina 'master' utilizziamo un controllo Repeater per eseguire il rendering di un elenco di categ...
ms.author: riande
ms.date: 10/30/2010
ms.assetid: 68b8c023-92fa-4df6-9563-1764e16e4b04
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-acess-two-pages-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: 6d4f38201eaa25097dcba589f35506204d3f73a3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829591"
---
<a name="masterdetail-filtering-across-two-pages-c"></a>Master/dettagli filtro su due pagine (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'App di esempio](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_34_CS.exe) o [Scarica il PDF](master-detail-filtering-acess-two-pages-datalist-cs/_static/datatutorial34cs1.pdf)

> In questa esercitazione verrà esaminata separare un report master o di dettaglio su due pagine. Nella pagina "master" utilizziamo un controllo Repeater per eseguire il rendering di un elenco di categorie che, quando selezionato, l'utente passerà alla pagina "Dettagli" in cui un controllo DataList di due colonne Mostra i prodotti appartenenti alla categoria selezionata.


## <a name="introduction"></a>Introduzione

Nel [esercitazione precedente](master-detail-filtering-with-a-dropdownlist-datalist-cs.md) è stato illustrato come visualizzare i report master o di dettaglio in una singola pagina web con controlli DropDownList per visualizzare il record "master" e un controllo DataList da visualizzare "Dettagli". Un altro modello comune usato per i report master o di dettaglio è avere i record master in un'unica pagina web e i dettagli su un altro. In precedenza [Master/Dettagli filtro in due pagine](../masterdetail/master-detail-filtering-across-two-pages-cs.md) esercitazione, sono stati esaminati questo modello Usa un controllo GridView per visualizzare tutti i fornitori nel sistema. Questo controllo GridView è incluso un HyperLinkField, che viene eseguito il rendering come collegamento a un'altra pagina, passando il `SupplierID` nella stringa di query. La seconda pagina utilizzato un GridView per elencare i prodotti specificati dal fornitore selezionato.

Tali report due pagine master/dettaglio può essere eseguito usando i controlli DataList e Repeater. L'unica differenza è che il controllo DataList né il controllo Repeater fornisce supporto per il controllo HyperLinkField. In alternativa, è necessario aggiungere un controllo Web collegamento ipertestuale o un elemento ancoraggio HTML (`<a>`) all'interno del controllo `ItemTemplate`. Il collegamento ipertestuale `NavigateUrl` proprietà o il punto di ancoraggio `href` attributo può essere personalizzato con gli approcci dichiarativi o a livello di codice.

In questa esercitazione verrà illustrato un esempio in cui sono elencate le categorie in un elenco puntato in una sola pagina con un controllo Repeater. Ogni elemento dell'elenco includerà nome e una descrizione della categoria con il nome della categoria visualizzato come collegamento a un'altra pagina. Facendo clic su questo collegamento verrà whisk all'utente la seconda pagina, in cui un controllo DataList mostrerà i prodotti appartenenti alla categoria selezionata.

## <a name="step-1-displaying-the-categories-in-a-bulleted-list"></a>Passaggio 1: Visualizzare le categorie in un elenco puntato

Il primo passaggio nella creazione di qualsiasi report master o di dettaglio è iniziare mediante la visualizzazione di record "master". Pertanto, la prima attività consiste nel visualizzare le categorie in pagina "master". Aprire il `CategoryListMaster.aspx` nella pagina di `DataListRepeaterFiltering` cartella, aggiungere un controllo Repeater e, dallo smart tag, scegliere di aggiungere un nuovo oggetto ObjectDataSource. Configurare il nuovo oggetto ObjectDataSource in modo che accede ai dati dal `CategoriesBLL` della classe `GetCategories` (metodo) (vedere la figura 1).


[![Configurare ObjectDataSource per l'uso GetCategories metodo della classe CategoriesBLL](master-detail-filtering-acess-two-pages-datalist-cs/_static/image2.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image1.png)

**Figura 1**: configurare ObjectDataSource per usare il `CategoriesBLL` della classe `GetCategories` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-acess-two-pages-datalist-cs/_static/image3.png))


Successivamente, definire i modelli di Repeater in modo che venga visualizzato ogni nome di categoria e una descrizione come un elemento in un elenco puntato. È possibile non ancora preoccuparsi della presenza di ogni categoria di collegamento alla pagina dei dettagli. Di seguito viene illustrato il markup dichiarativo per il controllo Repeater e ObjectDataSource:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample1.aspx)]

Con questo codice completato, si consiglia di visualizzare lo stato di avanzamento tramite un browser. Come illustrato nella figura 2, il controllo Repeater esegue il rendering come elenco puntato Mostra nome e descrizione di ogni categoria.


[![Ogni categoria viene visualizzata come elemento di elenco puntato](master-detail-filtering-acess-two-pages-datalist-cs/_static/image5.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image4.png)

**Figura 2**: ogni categoria viene visualizzata come elemento di elenco puntato ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-acess-two-pages-datalist-cs/_static/image6.png))


## <a name="step-2-turning-the-category-name-into-a-link-to-the-details-page"></a>Passaggio 2: Trasformare il nome della categoria in un collegamento alla pagina dei dettagli

Per consentire all'utente di visualizzare le informazioni di "Dettagli" per una determinata categoria, è necessario aggiungere un collegamento a ogni elenco puntato di elemento che, quando selezionato, richiederà all'utente la seconda pagina (`ProductsForCategoryDetails.aspx`). Questa seconda pagina visualizzerà quindi i prodotti per la categoria selezionata utilizzando un controllo DataList. Per determinare la categoria è stato fatto clic con il collegamento, è necessario passare la categoria selezionata `CategoryID` alla seconda pagina tramite un meccanismo. Il modo più semplice, più semplice per trasferire dati scalare da una pagina a altra è tramite la stringa di query, ovvero l'opzione che verrà usato in questa esercitazione. In particolare, il `ProductsForCategoryDetails.aspx` pagina previsti selezionata *`categoryID`* valore deve essere passato tramite un campo stringa di query denominato `CategoryID`. Ad esempio, per visualizzare i prodotti per la categoria di bevande, che ha un `CategoryID` pari a 1, un utente potrebbe visitare `ProductsForCategoryDetails.aspx?CategoryID=1`.

Per creare un collegamento ipertestuale per ogni elemento dell'elenco puntato in Repeater dobbiamo aggiungere un controllo Web collegamento ipertestuale o un elemento ancoraggio HTML (`<a>`) per il `ItemTemplate`. In scenari in cui il collegamento ipertestuale è visualizzato lo stesso per ogni riga, entrambi gli approcci è sufficiente. Per ripetitori, preferisco utilizzare l'elemento di ancoraggio. Per usare l'elemento di ancoraggio, aggiornare ItemTemplate il compito di Repeater:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample2.aspx)]

Si noti che il `CategoryID` può essere inserito direttamente all'interno dell'elemento di ancoraggio `href` attributo; tuttavia, per così essere determinate delimitare il `href` valore dell'attributo con (apostrofi e virgolette nota) dal `Eval` (metodo) all'interno di `href` attributo delimita la relativa stringa (`"CategoryID"`) tra virgolette. In alternativa, un controllo Web collegamento ipertestuale può essere usato invece:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample3.aspx)]

Si noti come la parte statica dell'URL, ovvero `ProductsForCategoryDetails.aspx?CategoryID` , viene aggiunto al risultato di `Eval("CategoryID")` direttamente all'interno di sintassi di associazione dati mediante la concatenazione di stringhe.

Uno dei vantaggi dell'utilizzo del controllo collegamento ipertestuale è che sia a livello di codice accessibile dal Repeater `ItemDataBound` gestore dell'evento, se necessario. È possibile, ad esempio visualizzare il nome della categoria come testo anziché come un collegamento per le categorie prodotti non associati. Tale controllo è stato possibile eseguire a livello di codice nel `ItemDataBound` gestore dell'evento; per le categorie senza prodotti associati, il collegamento ipertestuale del `NavigateUrl` proprietà può essere impostata su una stringa vuota, dando come risultato di quel determinato nome di categoria per il rendering come testo normale (anziché come un collegamento). Fare riferimento al [formattazione di DataList e Repeater in base al momento i dati](../displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-cs.md) esercitazione per altre informazioni sulla formattazione di DataList e del Repeater contenuto basato su logica a livello di codice tramite la `ItemDataBound` gestore dell'evento.

Se si segue, è possibile usare l'elemento di ancoraggio o approccio con il controllo collegamento ipertestuale nella pagina. Indipendentemente dall'approccio, quando si visualizza la pagina tramite un browser deve eseguire il rendering di ogni nome di categoria come collegamento alla pagina `ProductsForCategoryDetails.aspx`, passando l'applicabile `CategoryID` valore (vedere la figura 3).


[![I nomi delle categorie è ora collegata al ProductsForCategoryDetails.aspx](master-detail-filtering-acess-two-pages-datalist-cs/_static/image8.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image7.png)

**Figura 3**: la categoria nomi ora collegamento a `ProductsForCategoryDetails.aspx` ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-acess-two-pages-datalist-cs/_static/image9.png))


## <a name="step-3-listing-the-products-that-belong-to-the-selected-category"></a>Passaggio 3: Elenco dei prodotti che appartengono alla categoria selezionata

Con il `CategoryListMaster.aspx` pagina, completa, siamo pronti per concentrare l'attenzione che implementa la pagina "Dettagli", `ProductsForCategoryDetails.aspx`. Aprire questa pagina, trascinare un controllo DataList dalla casella degli strumenti nella finestra di progettazione e impostare relativi `ID` proprietà `ProductsInCategory`. Successivamente, smart tag del controllo DataList di scegliere di aggiungere un nuovo oggetto ObjectDataSource alla pagina, denominarlo `ProductsInCategoryDataSource`. Configurarlo in modo che chiami il `ProductsBLL` della classe `GetProductsByCategoryID(categoryID)` (metodo), impostare l'elenco a discesa sono elencati nelle schede INSERT, UPDATE e DELETE su (nessuno).


[![Configurare ObjectDataSource per l'uso GetProductsByCategoryID(categoryID) metodo della classe ProductsBLL](master-detail-filtering-acess-two-pages-datalist-cs/_static/image11.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image10.png)

**Figura 4**: configurare ObjectDataSource per usare il `ProductsBLL` della classe `GetProductsByCategoryID(categoryID)` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-acess-two-pages-datalist-cs/_static/image12.png))


Poiché il `GetProductsByCategoryID(categoryID)` metodo accetta un parametro di input (*`categoryID`*), la procedura guidata Scegli origine dati offre l'opportunità di specificare l'origine del parametro. Impostare l'origine del parametro QueryString utilizzando il QueryStringField `CategoryID`.


[![Usare il parametro CategoryID campo Querystring come origine del parametro](master-detail-filtering-acess-two-pages-datalist-cs/_static/image14.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image13.png)

**Figura 5**: usare Querystring Field `CategoryID` come origine del parametro ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-acess-two-pages-datalist-cs/_static/image15.png))


Come illustrato nelle esercitazioni precedenti, dopo aver completato la procedura guidata Scegli origine dati, Visual Studio crea automaticamente un `ItemTemplate` per il controllo DataList che elenca ogni nome del campo dati e il valore. Sostituire questo modello con uno che elenca solo nome del prodotto, fornitore e prezzo. Inoltre, impostare il controllo DataList `RepeatColumns` proprietà su 2. Dopo tali modifiche, markup dichiarativo di DataList e ObjectDataSource dovrebbe essere simile al seguente:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample4.aspx)]

Per visualizzare questa pagina in azione, iniziare dal `CategoryListMaster.aspx` pagina; successivamente, fare clic su un collegamento nell'elenco puntato di categorie. In questo modo, verrà `ProductsForCategoryDetails.aspx`, passando lungo il `CategoryID` tramite la stringa di query. Il `ProductsInCategoryDataSource` ObjectDataSource in `ProductsForCategoryDetails.aspx` verrà quindi visualizzato solo per tali prodotti per la categoria specificata e li visualizzano nella DataList, che esegue il rendering dei due prodotti per ogni riga. Figura 6 mostra una schermata di `ProductsForCategoryDetails.aspx` quando si visualizzano le bibite.


[![Vengono visualizzate le bibite, due per riga](master-detail-filtering-acess-two-pages-datalist-cs/_static/image17.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image16.png)

**Figura 6**: vengono visualizzate il bibite, due per ogni riga ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-acess-two-pages-datalist-cs/_static/image18.png))


## <a name="step-4-displaying-category-information-on-productsforcategorydetailsaspx"></a>Passaggio 4: Visualizzazione di informazioni di categoria su ProductsForCategoryDetails.aspx

Quando un utente fa clic su una categoria nel `CategoryListMaster.aspx`, vengono rilevati per `ProductsForCategoryDetails.aspx` e visualizzati i prodotti appartenenti alla categoria selezionata. Tuttavia, in `ProductsForCategoryDetails.aspx` non esistono Nessun visive riguardo a quale categoria è stata selezionata. Un utente che deve fare clic su bibite, ma fa clic accidentalmente su condimenti, non ha modo di realizzare gli errore dopo aver raggiunto `ProductsForCategoryDetails.aspx`. Per risolvere questo problema, è possibile visualizzare informazioni relative alla categoria selezionata, il nome e descrizione, ovvero all'inizio del `ProductsForCategoryDetails.aspx` pagina.

A tale scopo, aggiungere un controllo FormView sopra il controllo Repeater `ProductsForCategoryDetails.aspx`. Successivamente, aggiungere un nuovo oggetto ObjectDataSource alla pagina dallo smart tag del controllo FormView denominato `CategoryDataSource` e configurarlo per usare il `CategoriesBLL` della classe `GetCategoryByCategoryID(categoryID)` (metodo).


[![Accedere alle informazioni sulla categoria tramite metodo della classe GetCategoryByCategoryID(categoryID) CategoriesBLL](master-detail-filtering-acess-two-pages-datalist-cs/_static/image20.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image19.png)

**Figura 7**: accedere alle informazioni sulla categoria tramite la `CategoriesBLL` della classe `GetCategoryByCategoryID(categoryID)` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-acess-two-pages-datalist-cs/_static/image21.png))


Come con la `ProductsInCategoryDataSource` ObjectDataSource aggiunto nel passaggio 3, il `CategoryDataSource`della configurazione guidata origine dati richiede un'origine per il `GetCategoryByCategoryID(categoryID)` parametro di input del metodo. Usare le stesse identiche impostazioni come prima, impostando l'origine del parametro di stringa di query e il valore di QueryStringField su `CategoryID` (vedere la figura 5).

Dopo aver completato la procedura guidata, Visual Studio crea automaticamente un' `ItemTemplate`, `EditItemTemplate`, e `InsertItemTemplate` per FormView. Perché ti offriamo un'interfaccia di sola lettura, è possibile rimuovere il `EditItemTemplate` e `InsertItemTemplate`. Inoltre, è possibile personalizzare il controllo FormView `ItemTemplate`. Dopo avere rimosso i superflui modelli e personalizzazione ItemTemplate, markup dichiarativo di FormView e ObjectDataSource dovrebbe essere simile al seguente:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample5.aspx)]

Figura 8 mostra una schermata quando si visualizza questa pagina tramite un browser.

> [!NOTE]
> Oltre a FormView, ho aggiunto anche un controllo collegamento ipertestuale di sopra di FormView che indirizza l'utente torna all'elenco delle categorie (`CategoryListMaster.aspx`). È possibile inserire questo collegamento in un' posizione o per ometterlo completamente.


[![Informazioni sulle categorie viene ora visualizzata nella parte superiore della pagina](master-detail-filtering-acess-two-pages-datalist-cs/_static/image23.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image22.png)

**Figura 8**: informazioni sulle categorie viene ora visualizzata nella parte superiore della pagina ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-acess-two-pages-datalist-cs/_static/image24.png))


## <a name="step-5-displaying-a-message-if-no-products-belong-to-the-selected-category"></a>Passaggio 5: Visualizzazione di un messaggio se prodotti non appartengono alla categoria selezionata

Il `CategoryListMaster.aspx` pagina elenca tutte le categorie nel sistema, indipendentemente se sono presenti prodotti associati. Se un utente fa clic su una categoria con nessun prodotti associati, DataList in `ProductsForCategoryDetails.aspx` non vengono visualizzati, come la relativa origine dati non disporrà di tutti gli elementi. Come abbiamo visto nelle precedenti esercitazioni, GridView fornisce un `EmptyDataText` proprietà che può essere usata per specificare un messaggio di testo da visualizzare se non sono presenti record nell'origine dati. Sfortunatamente, nessuna delle due DataList e Repeater dispone di tale proprietà.

Per visualizzare un messaggio che informa l'utente che non sono presenti prodotti corrispondenti per la categoria selezionata, è necessario aggiungere un'etichetta di controllo alla pagina di cui `Text` proprietà viene assegnato il messaggio da visualizzare nel caso in cui non sono presenti prodotti corrispondenti. È quindi necessario impostare a livello di codice relativo `Visible` proprietà base o meno di DataList contiene elementi.

A tale scopo, iniziare aggiungendo un'etichetta sotto il controllo DataList. Impostare relativi `ID` proprietà `NoProductsMessage` e il relativo `Text` proprietà su "Non sono presenti prodotti per la categoria selezionata..." Successivamente, è necessario impostare a livello di codice questa etichetta `Visible` base o meno tutti i dati è stati associati alla proprietà di `ProductsInCategory` DataList. Questa assegnazione deve essere effettuata dopo aver associati i dati per il controllo DataList. Per il controllo GridView, DetailsView e FormView, è possibile creare un gestore eventi per il controllo `DataBound` evento, che viene attivato dopo l'associazione dati è stata completata. Tuttavia, DataList, né il Repeater ha un `DataBound` eventi disponibili.

Per questo particolare esempio è possibile assegnare l'etichetta `Visible` proprietà di `Page_Load` gestore eventi, poiché i dati verranno assegnati a DataList prima che la pagina `Load` evento. Tuttavia, questo approccio non funziona in generale, come i dati da ObjectDataSource sia associati a DataList in un secondo momento nel ciclo di vita della pagina. Ad esempio, se i dati visualizzati sono basati sul valore in un altro controllo, ad esempio si tratta la visualizzazione di un report master o di dettaglio usando un controllo DropDownList per contenere il record "master", i dati potrebbero riassociati non per il controllo Web per dati finché il `PreRender` fase nel ciclo di vita della pagina.

Una soluzione che funziona per tutti i casi consiste nell'assegnare il `Visible` proprietà `False` nella finestra di DataList `ItemDataBound` (o `ItemCreated`) gestore dell'evento quando si associa un tipo di elemento di `Item` o `AlternatingItem`. In questo caso sappiamo che vi sia dati almeno un elemento nell'origine dati e pertanto è possibile nascondere la `NoProductsMessage` etichetta. Oltre a questo gestore eventi, è necessario anche un gestore eventi per il controllo DataList `DataBinding` evento, in cui viene inizializzata l'etichetta `Visible` proprietà `True`. Poiché il `DataBinding` evento viene generato prima il `ItemDataBound` del eventi, l'etichetta `Visible` verrà inizialmente essere impostata su `True`; se sono presenti elementi di dati, tuttavia, verrà impostato `False`. Il codice seguente implementa la logica:

[!code-csharp[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample6.cs)]

Tutte le categorie nel database Northwind sono associate uno o più prodotti. Per testare questa funzionalità, ho regolato manualmente il database Northwind per questa esercitazione, la riassegnazione di tutti i prodotti associati alla categoria di prodotto (`CategoryID` = 7) per la categoria di frutti di mare (`CategoryID` = 8). Ciò può essere effettuata tramite Esplora Server scegliendo Nuova Query e con i seguenti `UPDATE` istruzione:

[!code-sql[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample7.sql)]

Dopo aver aggiornato il database di conseguenza, tornare al `CategoryListMaster.aspx` pagina e fare clic sul collegamento producono. Poiché non sono più presenti tutti i prodotti appartenenti alla categoria di prodotto, si dovrebbe vedere il messaggio "Non sono presenti prodotti per la categoria selezionata...", come illustrato nella figura 9.


[![Se sono presenti n prodotti appartenenti alla categoria selezionata viene visualizzato un messaggio](master-detail-filtering-acess-two-pages-datalist-cs/_static/image26.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image25.png)

**Figura 9**: viene visualizzato un messaggio se sono presenti n prodotti appartenenti alla categoria selezionata ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-acess-two-pages-datalist-cs/_static/image27.png))


## <a name="summary"></a>Riepilogo

Mentre i report master o di dettaglio è possono visualizzare i record master e di dettaglio in una singola pagina, in molti siti Web sono suddivisi in due pagine web. In questa esercitazione è stato illustrato come implementare questo rapporto master/dettaglio facendo in modo che le categorie elencate in un elenco puntato utilizzando un controllo Repeater nella pagina web "master" e i prodotti associati elencati nella pagina "Dettagli". Ogni elemento dell'elenco nella pagina master web contenuti un collegamento alla pagina dei dettagli passato lungo la riga `CategoryID` valore.

Nella pagina dei dettagli del recupero di tali prodotti per il fornitore specificato è stato eseguito tramite il `ProductsBLL` della classe `GetProductsByCategoryID(categoryID)` (metodo). Il *`categoryID`* valore del parametro è stato specificato in modo dichiarativo utilizzando il `CategoryID` valore querystring come origine del parametro. È stato anche illustrato come visualizzare i dettagli della categoria nella pagina dei dettagli con un controllo FormView e su come visualizzare un messaggio se si sono verificati senza prodotti appartenenti alla categoria selezionata.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali...

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. I revisori per questa esercitazione sono state Zack Jones e Liz Shulok. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](master-detail-filtering-with-a-dropdownlist-datalist-cs.md)
> [Successivo](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md)
