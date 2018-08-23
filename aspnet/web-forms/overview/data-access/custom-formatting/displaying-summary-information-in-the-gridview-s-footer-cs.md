---
uid: web-forms/overview/data-access/custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs
title: Visualizzazione di informazioni di riepilogo nel piè di pagina del controllo GridView (c#) | Microsoft Docs
author: rick-anderson
description: Le informazioni di riepilogo viene spesso visualizzate nella parte inferiore del report in una riga di riepilogo. Il controllo GridView può includere una riga di piè di pagina in cui le celle viene possibile delle richieste pull...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: d50edc31-9286-4c6a-8635-be09e72752a4
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs
msc.type: authoredcontent
ms.openlocfilehash: 76df8ea925f4485b52090723b2f0a37b25f7e684
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834033"
---
<a name="displaying-summary-information-in-the-gridviews-footer-c"></a>Visualizzazione di informazioni di riepilogo nel piè di pagina del controllo GridView (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'App di esempio](http://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_15_CS.exe) o [Scarica il PDF](displaying-summary-information-in-the-gridview-s-footer-cs/_static/datatutorial15cs1.pdf)

> Le informazioni di riepilogo viene spesso visualizzate nella parte inferiore del report in una riga di riepilogo. Il controllo GridView può includere una riga di piè di pagina in cui le celle a livello di codice è possibile inserire aggregare i dati. In questa esercitazione si noterà come visualizzare i dati aggregati in questa riga di piè di pagina.


## <a name="introduction"></a>Introduzione

Oltre a ognuno dei prezzi dei prodotti, le unità in magazzino, unità visualizzati in ordine e i livelli di riordino, un utente può anche essere interessati a informazioni di aggregazione, ad esempio il prezzo medio, il numero totale di unità a magazzino e così via. Tali informazioni di riepilogo viene spesso visualizzati nella parte inferiore del report in una riga di riepilogo. Il controllo GridView può includere una riga di piè di pagina in cui le celle a livello di codice è possibile inserire aggregare i dati.

Questa attività presenta tre problemi:

1. Configurazione di GridView per visualizzare la riga di piè di pagina
2. Determinare i dati di riepiloghi. vale a dire, come è calcolare il prezzo medio o il totale delle unità a magazzino?
3. Inserimento di dati di riepilogo in celle appropriate della riga di piè di pagina

In questa esercitazione vedremo come ovviare a questi problemi. In particolare, si creerà una pagina che elenca le categorie in un elenco a discesa con i prodotti della categoria selezionati visualizzati in un controllo GridView. Il controllo GridView includerà una riga di piè di pagina che mostra il prezzo medio e totale delle unità a magazzino e sull'ordine per i prodotti in tale categoria.


[![Le informazioni di riepilogo viene visualizzate nella riga di piè di pagina di GridView](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image2.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image1.png)

**Figura 1**: informazioni di riepilogo viene visualizzata nella riga di piè di pagina del controllo GridView ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image3.png))


Questa esercitazione, con relativa categoria all'interfaccia master/dettaglio prodotti, si basa sui concetti illustrati in precedenza [Master/Dettagli filtro con un controllo DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) esercitazione. Se si è già non ancora l'esercitazione precedente, farlo prima di proseguire con questo.

## <a name="step-1-adding-the-categories-dropdownlist-and-products-gridview"></a>Passaggio 1: Aggiunta di categorie DropDownList e prodotti GridView

Prima di noi stessi relative con l'aggiunta di informazioni di riepilogo al piè di pagina di GridView, prima di tutto semplicemente creiamo il report master o di dettaglio. Una volta che abbiamo completato il primo passaggio, esamineremo come includere i dati di riepilogo.

Iniziare aprendo il `SummaryDataInFooter.aspx` nella pagina di `CustomFormatting` cartella. Aggiungere un controllo DropDownList e impostare relativi `ID` a `Categories`. Successivamente, fare clic sul collegamento dallo smart tag del controllo DropDownList Scegli origine dati e scegliere di aggiungere un nuovo oggetto ObjectDataSource denominato `CategoriesDataSource` che richiama la `CategoriesBLL` della classe `GetCategories()` (metodo).


[![Aggiungere un nuovo oggetto ObjectDataSource denominato CategoriesDataSource](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image5.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image4.png)

**Figura 2**: aggiungere un nuovo oggetto ObjectDataSource denominato `CategoriesDataSource` ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image6.png))


[![Avere ObjectDataSource richiama metodo della classe GetCategories() CategoriesBLL](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image8.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image7.png)

**Figura 3**: il richiamare l'oggetto ObjectDataSource il `CategoriesBLL` della classe `GetCategories()` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image9.png))


Dopo la configurazione di ObjectDataSource, restituisce la procedura guidata di configurazione dell'origine dati del controllo DropDownList guidata dalla quale è necessario specificare quale valore del campo dati deve essere visualizzato e quello che deve corrispondere al valore del DropDownList `ListItem` s. Dispone il `CategoryName` campo visualizzato e l'utilizzo di `CategoryID` come valore.


[![Usare i campi di CategoryID e CategoryName come testo e il valore per gli elementi ListItem, rispettivamente](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image11.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image10.png)

**Figura 4**: usare il `CategoryName` e `CategoryID` i campi come il `Text` e `Value` per il `ListItem` s, rispettivamente ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image12.png))


A questo punto si dispone di un controllo DropDownList (`Categories`) che elenca le categorie nel sistema. È ora necessario aggiungere un controllo GridView in cui sono elencati i prodotti appartenenti alla categoria selezionata. Prima di procedere, tuttavia, si consiglia di controllare la casella di controllo Abilita AutoPostBack nello smart tag del controllo DropDownList. Come descritto nel *Master/Dettagli filtro con un controllo DropDownList* tutorial, impostando il controllo DropDownList `AutoPostBack` proprietà `true` la pagina verrà registrata nuovamente ogni volta che viene modificato il valore di DropDownList. In questo modo il controllo GridView per l'aggiornamento, che mostra i prodotti per la categoria appena selezionato. Se il `AutoPostBack` è impostata su `false` (impostazione predefinita), la modifica della categoria non causino un postback e pertanto non vengono aggiornate prodotti elencati.


[![La casella di AutoPostBack Enable nello Smart Tag del controllo DropDownList](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image14.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image13.png)

**Figura 5**: la casella Abilita AutoPostBack nello Smart Tag del controllo DropDownList ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image15.png))


Aggiungere un controllo GridView alla pagina per visualizzare i prodotti per la categoria selezionata. Impostare il controllo GridView `ID` al `ProductsInCategory` e associarlo a un nuovo oggetto ObjectDataSource denominato `ProductsInCategoryDataSource`.


[![Aggiungere un nuovo oggetto ObjectDataSource denominato ProductsInCategoryDataSource](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image17.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image16.png)

**Figura 6**: aggiungere un nuovo oggetto ObjectDataSource denominato `ProductsInCategoryDataSource` ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image18.png))


Configurare ObjectDataSource in modo che richiama il `ProductsBLL` della classe `GetProductsByCategoryID(categoryID)` (metodo).


[![Avere ObjectDataSource richiama il metodo GetProductsByCategoryID(categoryID)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image20.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image19.png)

**Figura 7**: il richiamare l'oggetto ObjectDataSource il `GetProductsByCategoryID(categoryID)` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image21.png))


Poiché il `GetProductsByCategoryID(categoryID)` metodo accetta un parametro di input, nel passaggio finale della procedura guidata è possibile specificare l'origine del valore del parametro. Per visualizzare i prodotti della categoria selezionata, hanno il parametro proveniente dal `Categories` DropDownList.


[![Ottenere il valore del parametro categoryID da DropDownList categorie selezionate](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image23.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image22.png)

**Figura 8**: Ottieni il *`categoryID`* valore del parametro da DropDownList categorie selezionate ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image24.png))


Dopo aver completato la procedura guidata GridView avrà un BoundField per ciascuna delle proprietà del prodotto. È possibile pulire questi BoundField in modo che solo le `ProductName`, `UnitPrice`, `UnitsInStock`, e `UnitsOnOrder` BoundField vengono visualizzati. È possibile aggiungere le impostazioni a livello di campo per i restanti BoundField (ad esempio la formattazione di `UnitPrice` come valuta). Dopo aver apportato queste modifiche, markup dichiarativo del controllo GridView dovrebbe essere simile al seguente:


[!code-aspx[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample1.aspx)]

A questo punto abbiamo un rapporto master/dettagli completamente funzionante che mostra il nome, prezzo unitario, unità a magazzino e unità sull'ordine per tali prodotti appartenenti alla categoria selezionata.


[![Ottenere il valore del parametro categoryID da DropDownList categorie selezionate](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image26.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image25.png)

**Figura 9**: Ottieni il *`categoryID`* valore del parametro da DropDownList categorie selezionate ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image27.png))


## <a name="step-2-displaying-a-footer-in-the-gridview"></a>Passaggio 2: Visualizzare un piè di pagina in GridView

Il controllo GridView può visualizzare righe sia un'intestazione e piè di pagina. Vengono visualizzate queste righe a seconda dei valori del `ShowHeader` e `ShowFooter` delle proprietà, rispettivamente, con `ShowHeader` verrà utilizzato `true` e `ShowFooter` a `false`. Per includere un piè di pagina in GridView è sufficiente impostare relativi `ShowFooter` proprietà `true`.


[![Impostare la proprietà del controllo GridView ShowFooter su true](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image29.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image28.png)

**Figura 10**: impostare il controllo GridView `ShowFooter` proprietà `true` ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image30.png))


La riga di piè di pagina dispone di una cella per ognuno dei campi definiti in GridView; Tuttavia, queste celle sono vuote per impostazione predefinita. Si consiglia di visualizzare lo stato di avanzamento in un browser. Con il `ShowFooter` ora impostata su `true`, GridView include una riga di piè di pagina vuota.


[![L'ora di GridView include una riga di piè di pagina](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image32.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image31.png)

**Figura 11**: il controllo GridView include ora una riga di piè di pagina ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image33.png))


La riga di piè di pagina nella figura 11 non spiccano perché ha uno sfondo bianco. È possibile creare un `FooterStyle` della classe CSS in `Styles.css` che specifica uno sfondo rosso scuro e quindi configurare il `GridView.skin` soubor skinu nel `DataWebControls` tema per assegnare questa classe CSS per il controllo GridView `FooterStyle`del `CssClass` proprietà. Se è necessario perfezionare le interfacce e temi, vedere la [visualizzazione di dati con ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) esercitazione.

Iniziare aggiungendo la seguente classe CSS da `Styles.css`:


[!code-css[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample2.css)]

Il `FooterStyle` classe CSS è simile in stile per il `HeaderStyle` classe, anche se il `HeaderStyle`del colore di sfondo è importante sottolineare che più scuro e viene visualizzato il testo in grassetto. Inoltre, il testo nel piè di pagina è allineato a destra mentre il testo dell'intestazione è centrato.

Successivamente, per associare questa classe CSS a piè di pagina ogni di GridView, aprire il `GridView.skin` del file nel `DataWebControls` tema e impostare il `FooterStyle`del `CssClass` proprietà. Dopo l'aggiunta markup del file simile a:


[!code-aspx[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample3.aspx)]

Come nell'immagine riportata di seguito viene illustrata, questa modifica rende il piè di pagina maggiore risalto chiaramente.


[![Riga di piè di pagina di GridView ha ora un colore di sfondo rossastri](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image35.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image34.png)

**Figura 12**: riga di piè di pagina di GridView The ha ora un colore di sfondo rossastri ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image36.png))


## <a name="step-3-computing-the-summary-data"></a>Passaggio 3: Calcolo i dati di riepilogo

Con piè di pagina di GridView visualizzato, la sfida successiva che ci viene illustrato come calcolare i dati di riepilogo. Esistono due modi per calcolare queste informazioni di aggregazione:

1. Tramite una query SQL è stato possibile eseguire un'ulteriore query al database per calcolare i dati riepilogativi per una determinata categoria. SQL include un numero di funzioni di aggregazione con un `GROUP BY` clausola per specificare i dati su cui devono essere riepilogati i dati. La query SQL seguente potrebbe riportare le informazioni necessarie:  

    [!code-sql[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample4.sql)]

    Naturalmente è preferibile non eseguire questa query direttamente dai `SummaryDataInFooter.aspx` pagina, ma piuttosto mediante la creazione di un metodo nel `ProductsTableAdapter` e il `ProductsBLL`.
2. Queste informazioni di calcolo come viene aggiunto a GridView come descritto in [formattazione basata su dati personalizzati](custom-formatting-based-upon-data-cs.md) esercitazioni, GridView `RowDataBound` gestore dell'evento viene generato una volta per ogni riga viene aggiunta a GridView dopo il relativo stato con associazione a dati. Tramite la creazione di un gestore eventi per questo evento possiamo così conservare un in esecuzione totale di valori che si desidera aggregare. Dopo l'ultima riga di dati è stata associata a GridView abbiamo i totali e le informazioni necessarie per calcolare la Media.

Il secondo approccio in genere si utilizza come Salva una corsa nel database e lo sforzo necessario per implementare la funzionalità di riepilogo nel livello di accesso ai dati e Business Logic Layer, ma sarebbe sufficiente entrambi gli approcci. Per questa esercitazione è possibile usare la seconda opzione e tenere traccia del totale parziale usando il `RowDataBound` gestore dell'evento.

Creare un `RowDataBound` gestore dell'evento per il controllo GridView selezionando il controllo GridView nella finestra di progettazione, scegliendo l'icona a forma di fulmine dalla finestra delle proprietà e fare doppio clic su di `RowDataBound` evento. Verrà creato un nuovo gestore eventi denominato `ProductsInCategory_RowDataBound` nella `SummaryDataInFooter.aspx` classe code-behind della pagina.


[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample5.cs)]

Per annotare un totale parziale, è necessario definire le variabili all'esterno dell'ambito del gestore dell'evento. Creare le seguenti quattro variabili a livello di pagina:

- `_totalUnitPrice`, di tipo `decimal`
- `_totalNonNullUnitPriceCount`, di tipo `int`
- `_totalUnitsInStock`, di tipo `int`
- `_totalUnitsOnOrder`, di tipo `int`

Successivamente, scrivere il codice per incrementare queste tre variabili per ogni riga di dati rilevati nel `RowDataBound` gestore dell'evento.


[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample6.cs)]

Il `RowDataBound` gestore dell'evento viene avviato, garantendo che stiamo affrontando una DataRow. Dopo che è stata stabilita, il `Northwind.ProductsRow` istanza in cui è stata appena associata ai `GridViewRow` dell'oggetto `e.Row` viene archiviato nella variabile `product`. Le variabili totale successive, in esecuzione vengono incrementate di valori corrispondenti del prodotto corrente (presupponendo che non contengono un database `NULL` valore). Viene tenuta traccia dell'esecuzione di entrambe `UnitPrice` totale e il numero di non -`NULL` `UnitPrice` registra perché il prezzo medio è il quoziente di questi due numeri.

## <a name="step-4-displaying-the-summary-data-in-the-footer"></a>Passaggio 4: Visualizzazione dei dati di riepilogo nel piè di pagina

Con i dati di riepilogo calcolato il totale, l'ultimo passaggio consiste di visualizzarla nella riga di piè di pagina di GridView. Questa attività, troppo, può essere eseguita a livello di programmazione tramite le `RowDataBound` gestore dell'evento. Si tenga presente che il `RowDataBound` gestore dell'evento viene generato per *ogni* riga in cui è associato a GridView, inclusa la riga di piè di pagina. Di conseguenza, è possibile aumentare nostro gestore di eventi per visualizzare i dati della riga di piè di pagina usando il codice seguente:


[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample7.cs)]

Poiché la riga di piè di pagina viene aggiunta a GridView dopo l'aggiunta di tutte le righe di dati, possiamo essere certi che nel momento siamo pronti per visualizzare i dati di riepilogo nel piè di pagina che verranno completati i calcoli totali parziale. L'ultimo passaggio, quindi, consiste nell'impostare questi valori nelle celle del piè di pagina.

Per visualizzare il testo in una cella del piè di pagina specifico, usare `e.Row.Cells[index].Text = value`, dove il `Cells` indicizzazione inizia in corrispondenza di 0. Il codice seguente calcola il prezzo medio (il prezzo totale diviso per il numero di prodotti) e visualizzarlo insieme al numero totale di unità in magazzino e quantità ordinata nelle celle di un piè di pagina appropriate di GridView.


[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample8.cs)]

Figura 13 Mostra il report dopo l'aggiunta di questo codice. Si noti come il `ToString("c")` fa sì che le informazioni di riepilogo prezzo medio di essere formattato come una valuta.


[![Riga di piè di pagina di GridView ha ora un colore di sfondo rossastri](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image38.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image37.png)

**Figura 13**: riga di piè di pagina di GridView The ha ora un colore di sfondo rossastri ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image39.png))


## <a name="summary"></a>Riepilogo

Visualizzazione Riepilogo dei dati è un requisito comune per report e il controllo GridView semplifica includere tali informazioni nella relativa riga di piè di pagina. La riga di piè di pagina viene visualizzata quando GridView `ShowFooter` è impostata su `true` e può includere il testo nelle celle impostate a livello di programmazione tramite le `RowDataBound` gestore dell'evento. I dati di riepilogo di computing uno scopo eseguendo nuovamente una query del database o tramite il codice nella classe code-behind della pagina ASP.NET per il calcolo a livello di codice i dati di riepilogo.

Questa esercitazione è terminata l'esame della formattazione personalizzata con i controlli GridView, DetailsView e FormView. Seguire l'esercitazione successiva viene avviato l'esplorazione di inserimento, aggiornamento ed eliminazione di dati tramite questi controlli.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Precedente](using-the-formview-s-templates-cs.md)
> [Successivo](custom-formatting-based-upon-data-vb.md)
