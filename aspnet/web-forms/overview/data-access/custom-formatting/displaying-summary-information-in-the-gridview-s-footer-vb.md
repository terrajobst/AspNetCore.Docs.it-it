---
uid: web-forms/overview/data-access/custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb
title: Visualizzazione di informazioni di riepilogo nel piè di pagina del controllo GridView (VB) | Documenti Microsoft
author: rick-anderson
description: Le informazioni di riepilogo viene spesso visualizzate nella parte inferiore del report in una riga di riepilogo. Il controllo GridView può includere una riga di piè di pagina in cui le celle è possibile pr...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 41c818b7-603a-402b-8847-890a63547b6f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb
msc.type: authoredcontent
ms.openlocfilehash: d9a1a3f3c680f367395f984254da6cdcdd3c08d4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="displaying-summary-information-in-the-gridviews-footer-vb"></a>Visualizzazione di informazioni di riepilogo nel piè di pagina del controllo GridView (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare App di esempio](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_15_VB.exe) o [Scarica il PDF](displaying-summary-information-in-the-gridview-s-footer-vb/_static/datatutorial15vb1.pdf)

> Le informazioni di riepilogo viene spesso visualizzate nella parte inferiore del report in una riga di riepilogo. Il controllo GridView può includere una riga di piè di pagina in cui le celle a livello di codice è possibile inserire dati aggregati. In questa esercitazione vedremo come visualizzare dati aggregati in questa riga di piè di pagina.


## <a name="introduction"></a>Introduzione

Oltre a visualizzare i prezzi dei prodotti, le unità in magazzino, unità, ordine e livelli di riordino, un utente potrebbe anche essere interessante informazioni di aggregazione, ad esempio il prezzo medio, il numero totale di unità in magazzino e così via. Tali informazioni di riepilogo viene spesso visualizzati nella parte inferiore del report in una riga di riepilogo. Il controllo GridView può includere una riga di piè di pagina in cui le celle a livello di codice è possibile inserire dati aggregati.

Questa attività presenta tre problemi:

1. Configurazione di GridView per visualizzare la riga di piè di pagina
2. Determinazione dei dati di riepilogo. vale a dire, come si calcola il prezzo medio o il totale delle unità in magazzino?
3. Inserire i dati di riepilogo in celle appropriate della riga di piè di pagina

In questa esercitazione vedremo come ovviare a questi problemi. In particolare, si creerà una pagina che elenca le categorie in un elenco a discesa con i prodotti della categoria selezionata in GridView. GridView includerà una riga di piè di pagina che mostra il prezzo medio e il numero totale di unità in magazzino e sull'ordine di prodotti in tale categoria.


[![Le informazioni di riepilogo viene visualizzate nella riga di piè di pagina del controllo GridView](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image2.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image1.png)

**Figura 1**: informazioni di riepilogo viene visualizzata nella riga di piè di pagina del controllo GridView ([fare clic per visualizzare l'immagine ingrandita](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image3.png))


Questa esercitazione, con la categoria all'interfaccia master-details prodotti, si basa i concetti trattati in precedenza [Master-Details filtro con un DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) esercitazione. Se è lavorato non ancora l'esercitazione precedente, farlo prima di proseguire con questo.

## <a name="step-1-adding-the-categories-dropdownlist-and-products-gridview"></a>Passaggio 1: Aggiunta di categorie DropDownList e prodotti GridView

Prima della relativa effettuata con l'aggiunta di informazioni di riepilogo al piè di pagina del controllo GridView, prima semplicemente creare ora il report master-details. Una volta che abbiamo completato il primo passaggio, verrà esaminato come includere dati di riepilogo.

Aprire il `SummaryDataInFooter.aspx` nella pagina di `CustomFormatting` cartella. Aggiungere un controllo DropDownList e impostare il relativo `ID` a `Categories`. Fare quindi clic sul collegamento smart tag del DropDownList Scegli origine dati e scegliere di aggiungere un nuovo oggetto ObjectDataSource denominato `CategoriesDataSource` che richiama la `CategoriesBLL` della classe `GetCategories()` metodo.


[![Aggiungere un nuovo oggetto ObjectDataSource denominato CategoriesDataSource](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image5.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image4.png)

**Figura 2**: aggiungere un nuovo denominato ObjectDataSource `CategoriesDataSource` ([fare clic per visualizzare l'immagine ingrandita](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image6.png))


[![Avere ObjectDataSource GetCategories() metodo della classe CategoriesBLL Invoke](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image8.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image7.png)

**Figura 3**: il ObjectDataSource richiamare il `CategoriesBLL` della classe `GetCategories()` metodo ([fare clic per visualizzare l'immagine ingrandita](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image9.png))


Dopo aver configurato il ObjectDataSource, verrà nuovamente la configurazione dell'origine dati del DropDownList guidata da cui è necessario specificare quale valore del campo dati deve essere visualizzato e quale deve corrispondere al valore del DropDownList `ListItem` s. Dispone il `CategoryName` campo visualizzato e l'utilizzo di `CategoryID` come valore.


[![Utilizzare i campi di CategoryID e CategoryName come testo e il valore per gli elementi ListItem presenti, rispettivamente](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image11.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image10.png)

**Figura 4**: usare il `CategoryName` e `CategoryID` i campi come il `Text` e `Value` per il `ListItem` s, rispettivamente ([fare clic per visualizzare l'immagine ingrandita](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image12.png))


A questo punto si dispone di un controllo DropDownList (`Categories`) che sono elencate le categorie nel sistema. È ora necessario aggiungere un controllo GridView in cui sono elencati i prodotti appartenenti alla categoria selezionata. Prima di effettuare, tuttavia, è opportuno controllare la casella di controllo Abilita un postback automatico smart tag del DropDownList. Come descritto nel *Master-Details filtro con un DropDownList* tutorial, impostando il DropDownList `AutoPostBack` proprietà `True` la pagina verrà registrata nuovamente ogni volta che viene modificato il valore di DropDownList. In questo modo GridView l'aggiornamento, che mostra i prodotti per la categoria selezionata. Se il `AutoPostBack` è impostata su `False` (impostazione predefinita), la modifica della categoria non provocheranno un postback e pertanto non vengono aggiornate prodotti elencati.


[![La casella di un postback automatico Abilita del DropDownList Smart tag](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image14.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image13.png)

**Figura 5**: la casella Abilita un postback automatico del DropDownList Smart tag ([fare clic per visualizzare l'immagine ingrandita](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image15.png))


Aggiungere un controllo GridView alla pagina per visualizzare i prodotti per la categoria selezionata. Impostare il controllo GridView `ID` a `ProductsInCategory` e associarlo a un nuovo oggetto ObjectDataSource denominato `ProductsInCategoryDataSource`.


[![Aggiungere un nuovo oggetto ObjectDataSource denominato ProductsInCategoryDataSource](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image17.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image16.png)

**Figura 6**: aggiungere un nuovo denominato ObjectDataSource `ProductsInCategoryDataSource` ([fare clic per visualizzare l'immagine ingrandita](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image18.png))


Configurare ObjectDataSource in modo che richiama la `ProductsBLL` della classe `GetProductsByCategoryID(categoryID)` metodo.


[![Avere ObjectDataSource richiamare il metodo GetProductsByCategoryID(categoryID)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image20.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image19.png)

**Figura 7**: il ObjectDataSource richiamare il `GetProductsByCategoryID(categoryID)` metodo ([fare clic per visualizzare l'immagine ingrandita](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image21.png))


Poiché il `GetProductsByCategoryID(categoryID)` metodo accetta un parametro di input, il passaggio finale della procedura guidata è possibile specificare l'origine del valore del parametro. Per visualizzare i prodotti della categoria selezionata, sono estratto dal parametro di `Categories` DropDownList.


[![Ottenere il valore del parametro categoryID da DropDownList categorie selezionate](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image23.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image22.png)

**Figura 8**: ottenere il *`categoryID`* valore del parametro da DropDownList categorie selezionate ([fare clic per visualizzare l'immagine ingrandita](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image24.png))


Dopo aver completato la procedura guidata GridView avrà un BoundField per ognuna delle proprietà del prodotto. Consente di pulire questi BoundField in modo che solo il `ProductName`, `UnitPrice`, `UnitsInStock`, e `UnitsOnOrder` BoundField vengono visualizzati. È possibile aggiungere le impostazioni a livello di campo per restanti BoundField (ad esempio la formattazione di `UnitPrice` come valuta). Dopo aver apportato queste modifiche, markup dichiarativo del controllo GridView dovrebbe essere simile al seguente:


[!code-aspx[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample1.aspx)]

A questo punto si dispone di un report master-details completamente funzionante che mostra il nome, il prezzo unitario, scorte e unità ordinate per i prodotti appartenenti alla categoria selezionata.


[![Ottenere il valore del parametro categoryID da DropDownList categorie selezionate](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image26.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image25.png)

**Figura 9**: ottenere il *`categoryID`* valore del parametro da DropDownList categorie selezionate ([fare clic per visualizzare l'immagine ingrandita](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image27.png))


## <a name="step-2-displaying-a-footer-in-the-gridview"></a>Passaggio 2: Visualizzare un piè di pagina in GridView

Il controllo GridView è possibile visualizzare righe di intestazione e piè di pagina. Queste righe vengono visualizzate in base ai valori del `ShowHeader` e `ShowFooter` proprietà, rispettivamente, con `ShowHeader` l'impostazione `True` e `ShowFooter` a `False`. Per includere semplicemente un piè di pagina in GridView, impostare il relativo `ShowFooter` proprietà `True`.


[![Impostare la proprietà del controllo GridView ShowFooter su True](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image29.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image28.png)

**Figura 10**: impostare il controllo GridView `ShowFooter` proprietà `True` ([fare clic per visualizzare l'immagine ingrandita](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image30.png))


La riga di piè di pagina dispone di una cella per ognuno dei campi definiti in GridView; le celle vengono, tuttavia, per impostazione predefinita. Richiedere qualche istante per visualizzare l'avanzamento in un browser. Con il `ShowFooter` ora impostata su `True`, GridView include una riga di piè di pagina vuota.


[![L'ora di GridView include una riga di piè di pagina](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image32.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image31.png)

**Figura 11**: GridView ora include una riga di piè di pagina ([fare clic per visualizzare l'immagine ingrandita](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image33.png))


La riga di piè di pagina nella figura 11 non evidenziati, perché contiene uno sfondo bianco. Creare un `FooterStyle` della classe CSS in `Styles.css` che specifica uno sfondo rosso scuro e quindi configurare il `GridView.skin` file dell'interfaccia di `DataWebControls` tema per assegnare la classe CSS per il controllo GridView `FooterStyle`del `CssClass` proprietà. Se si desidera ripassare interfacce e temi, fare riferimento in futuro il [la visualizzazione di dati con ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) esercitazione.

Per iniziare, aggiungere la seguente classe CSS `Styles.css`:


[!code-css[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample2.css)]

Il `FooterStyle` classe CSS è simile in stile per il `HeaderStyle` classe, sebbene il `HeaderStyle`del colore di sfondo è abbastanza particolare più scuro e viene visualizzato il testo in grassetto. Inoltre, il testo nel piè di pagina è allineata a destra mentre il testo dell'intestazione viene centrato.

Per associare questa classe CSS nella piè di pagina ogni GridView, aprire quindi il `GridView.skin` file nel `DataWebControls` tema e impostare il `FooterStyle`del `CssClass` proprietà. Dopo l'aggiunta markup del file simile a:


[!code-aspx[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample3.aspx)]

Come nell'immagine riportata di seguito viene illustrato, questa modifica rende il piè di pagina maggior risalto.


[![Riga di piè di pagina del controllo GridView ora ha un colore di sfondo rossastri](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image35.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image34.png)

**Figura 12**: riga di piè di pagina di GridView ora ha un colore di sfondo rossastri ([fare clic per visualizzare l'immagine ingrandita](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image36.png))


## <a name="step-3-computing-the-summary-data"></a>Passaggio 3: Calcolo i dati di riepilogo

Con piè di pagina del controllo GridView visualizzato, la sfida successiva che ci viene illustrato come calcolare i dati di riepilogo. Esistono due modi per calcolare queste informazioni di aggregazione:

1. Tramite una query SQL, è possibile rilasciare un'ulteriore query al database per i dati di riepilogo per una particolare categoria di calcolo. SQL include una serie di funzioni di aggregazione insieme a un `GROUP BY` clausola per specificare i dati su cui devono essere sono riepilogati i dati. La seguente query SQL potrebbe riportare le informazioni necessarie:  

    [!code-sql[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample4.sql)]

    Naturalmente è preferibile eseguire questa query direttamente dal `SummaryDataInFooter.aspx` , bensì mediante la creazione di un metodo nella pagina di `ProductsTableAdapter` e `ProductsBLL`.
2. Queste informazioni di calcolo, come viene aggiunto a GridView come descritto in [formattazione basato su dati personalizzati](custom-formatting-based-upon-data-cs.md) , GridView dell'esercitazione `RowDataBound` gestore dell'evento viene generato una volta per ogni riga viene aggiunta a GridView dopo il relativo stato associazione a dati. Tramite la creazione di un gestore eventi per questo evento è possibile mantenere in esecuzione totale dei valori di cui si desidera aggregare. Dopo l'ultima riga di dati è stata associata a GridView sono presenti i totali e le informazioni necessarie per calcolare la Media.

Il secondo approccio in genere si utilizza come Salva un andata e ritorno al database e l'impegno necessario per implementare la funzionalità di riepilogo nel livello di accesso ai dati e il livello di logica di Business, ma dovrebbe essere sufficiente a entrambi gli approcci. Per questa esercitazione verrà utilizzata la seconda opzione e tenere traccia del totale parziale usando il `RowDataBound` gestore dell'evento.

Creare un `RowDataBound` gestore eventi per il controllo GridView selezionando GridView nella finestra di progettazione, fare clic sull'icona del lampo dalla finestra delle proprietà e fare doppio clic su di `RowDataBound` evento. In alternativa, è possibile selezionare l'oggetto GridView con il relativo evento RowDataBound dagli elenchi a discesa nella parte superiore del file di classe code-behind di ASP.NET. Verrà creato un nuovo gestore eventi denominato `ProductsInCategory_RowDataBound` nel `SummaryDataInFooter.aspx` classe code-behind della pagina.


[!code-vb[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample5.vb)]

Per annotare un totale parziale è necessario definire le variabili all'esterno dell'ambito del gestore dell'evento. Creare le variabili a livello di pagina quattro seguenti:

- `_totalUnitPrice`, di tipo `Decimal`
- `_totalNonNullUnitPriceCount`, di tipo `Integer`
- `_totalUnitsInStock`, di tipo `Integer`
- `_totalUnitsOnOrder`, di tipo `Integer`

Successivamente, scrivere il codice per incrementare le tre variabili per ogni riga di dati rilevati nel `RowDataBound` gestore dell'evento.


[!code-vb[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample6.vb)]

Il `RowDataBound` gestore eventi viene avviato, verificare che ci stiamo occupa un DataRow. Una volta che è stata stabilita, il `Northwind.ProductsRow` istanza che è stata associata solo al `GridViewRow` oggetto `e.Row` viene archiviato nella variabile `product`. Quindi, in esecuzione variabili totale vengono incrementate di valori corrispondenti del prodotto corrente (supponendo che non contengono un database `NULL` valore). Microsoft tiene traccia dell'entrambe in esecuzione il `UnitPrice` totale e il numero di non`NULL` `UnitPrice` registra poiché il prezzo medio è il quoziente di questi due numeri.

## <a name="step-4-displaying-the-summary-data-in-the-footer"></a>Passaggio 4: Visualizzare i dati di riepilogo nel piè di pagina

Con i dati di riepilogo sommati, l'ultimo passaggio consiste per visualizzarlo nella riga di piè di pagina del controllo GridView. Questa attività, può essere eseguita a livello di programmazione tramite le `RowDataBound` gestore dell'evento. Tenere presente che il `RowDataBound` gestore dell'evento viene generato per *ogni* riga in cui è associato a GridView, inclusa la riga di piè di pagina. Pertanto, è possibile aumentare il gestore eventi per visualizzare i dati nella riga di piè di pagina utilizzando il codice seguente:


[!code-vb[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample7.vb)]

Poiché la riga di piè di pagina viene aggiunta al controllo GridView. dopo aver aggiunto tutte le righe di dati, è possibile avere la certezza che quando si è pronti per visualizzare i dati di riepilogo nel piè di pagina che verranno completati i calcoli totali parziale. L'ultimo passaggio, è quindi, per impostare tali valori nelle celle del piè di pagina.

Per visualizzare testo in una cella particolare piè di pagina, utilizzare `e.Row.Cells(index).Text = value`, dove il `Cells` indicizzazione inizia da 0. Il codice seguente calcola il prezzo medio (il prezzo totale diviso per il numero di prodotti) e visualizzarlo insieme al numero totale di unità in magazzino e quantità ordinata nelle celle del piè di pagina appropriate di GridView.


[!code-vb[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample8.vb)]

Figura 13 viene mostrato il report dopo l'aggiunta di questo codice. Si noti come `ToString("c")` fa sì che le informazioni di riepilogo del prezzo medio di essere formattato come una valuta.


[![Riga di piè di pagina del controllo GridView ora ha un colore di sfondo rossastri](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image38.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image37.png)

**Figura 13**: riga di piè di pagina di GridView ora ha un colore di sfondo rossastri ([fare clic per visualizzare l'immagine ingrandita](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image39.png))


## <a name="summary"></a>Riepilogo

Visualizzazione di dati di riepilogo è un requisito comune di report e il controllo GridView semplifica comprendono informazioni quali la riga di piè di pagina. La riga di piè di pagina viene visualizzata quando il controllo GridView `ShowFooter` è impostata su `True` e può avere il testo nelle celle impostate a livello di codice tramite la `RowDataBound` gestore dell'evento. Elaborazione dati di riepilogo uno scopo eseguendo nuovamente il database o tramite il codice nella classe code-behind della pagina ASP.NET per il calcolo a livello di programmazione i dati di riepilogo.

Questa esercitazione si conclude l'esame di formattazione personalizzata con i controlli di GridView, DetailsView e FormView. Esercitazione successiva avvia l'esplorazione di inserimento, aggiornamento ed eliminazione di dati tramite questi controlli.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Precedente](using-the-formview-s-templates-vb.md)
