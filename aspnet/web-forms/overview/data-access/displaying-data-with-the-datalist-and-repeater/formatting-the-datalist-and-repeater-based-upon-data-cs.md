---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-cs
title: Formattazione di DataList e Repeater sulla base dei dati (c#) | Documenti Microsoft
author: rick-anderson
description: In questa esercitazione verrà esaminare esempi di come si formatta l'aspetto dei controlli DataList e Repeater, tramite l'utilizzo di funzioni di formattazione con...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: 83e3d759-82b8-41e6-8d62-f0f4b3edec41
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 00dac460ad905d34632bca3249e019ddc626e440
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30876215"
---
<a name="formatting-the-datalist-and-repeater-based-upon-data-c"></a>Formattazione di DataList e Repeater sulla base dei dati (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare App di esempio](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_30_CS.exe) o [Scarica il PDF](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/datatutorial30cs1.pdf)

> In questa esercitazione verrà esaminare esempi di come si formatta l'aspetto dei controlli DataList e Repeater tramite funzioni formattazione all'interno di modelli o la gestione dell'evento associato a dati.


## <a name="introduction"></a>Introduzione

Come illustrato nell'esercitazione precedente, DataList offre una serie di proprietà correlate allo stile che influiscono sull'aspetto. In particolare, è stato illustrato come assegnare predefinito classi CSS DataList s `HeaderStyle`, `ItemStyle`, `AlternatingItemStyle`, e `SelectedItemStyle` proprietà. Oltre a queste quattro proprietà DataList include una serie di altre proprietà correlate allo stile, ad esempio `Font`, `ForeColor`, `BackColor`, e `BorderWidth`, solo per citarne alcune. Il controllo Repeater non contiene le proprietà correlate allo stile. Tali impostazioni di stile devono essere resa direttamente all'interno del markup nei modelli s Repeater.

Spesso, tuttavia, come devono essere formattati i dati varia a seconda dei dati stessi. Ad esempio, quando si elencano i prodotti potrebbe essere opportuno visualizzare le informazioni sul prodotto in un colore grigio chiaro se non è più disponibile o si può decidere di evidenziare il `UnitsInStock` valore se è zero. Come abbiamo visto nelle esercitazioni precedenti, il controllo GridView, DetailsView e FormView offrono due modi diversi per formattare l'aspetto in base ai relativi dati:

- **Il `DataBound` evento** creare un gestore eventi per l'oggetto appropriato `DataBound` evento, che viene attivato dopo aver associati i dati per ogni elemento (per GridView era il `RowDataBound` evento; per il DataList e Repeater è il `ItemDataBound`evento). In tal caso gestore, solo i dati associati può essere esaminati e formattazione decisioni apportate. Abbiamo esaminato questa tecnica nel [formattazione basato su dati personalizzati](../custom-formatting/custom-formatting-based-upon-data-cs.md) esercitazione.
- **Le funzioni nei modelli di formattazione** quando si utilizza TemplateFields i controlli di DetailsView o GridView o un modello nel controllo FormView, è possibile aggiungere una funzione di formattazione per la classe code-behind pagina s ASP.NET, il livello di logica di Business o qualsiasi altra libreria di classi che è accessibile dall'applicazione web. Questa funzione formattazione può accettare un numero arbitrario di parametri di input, ma deve restituire il codice HTML per eseguire il rendering nel modello. Funzioni di formattazione sono stati prima esaminate nel [TemplateFields utilizzo nel controllo GridView](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) esercitazione.

Entrambe queste tecniche di formattazione disponibili con i controlli DataList e Repeater. In questa esercitazione verrà esaminare esempi di utilizzo di entrambe le tecniche per entrambi i controlli.

## <a name="using-theitemdataboundevent-handler"></a>Utilizzo di`ItemDataBound`gestore dell'evento

Quando dati sono associati a un controllo DataList, da un controllo origine dati o mediante l'assegnazione a livello di programmazione di dati al controllo s `DataSource` proprietà e chiamando il relativo `DataBind()` (metodo), s DataList `DataBinding` evento viene generato, l'origine di dati enumerato, e ogni record di dati è associato al controllo DataList. Per ogni record nell'origine dati, crea DataList un [ `DataListItem` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistitem.aspx) oggetto quindi associato al record corrente. Durante questo processo, DataList genera due eventi:

- **`ItemCreated`** viene generato dopo il `DataListItem` è stato creato
- **`ItemDataBound`** viene generato dopo il record corrente è stato associato per il `DataListItem`

Di seguito viene illustrato il processo di associazione dati per il controllo DataList.

1. DataList s [ `DataBinding` evento](https://msdn.microsoft.com/library/system.web.ui.control.databinding.aspx) generato
2. L'associazione dati per il controllo DataList  
  
   Per ogni record nell'origine dati 

    1. Creare un `DataListItem` oggetto
    2. Attivare il [ `ItemCreated` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.itemcreated.aspx)
    3. Associare il record per il `DataListItem`
    4. Attivare il [ `ItemDataBound` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.itemdatabound.aspx)
    5. Aggiungere il `DataListItem` per il `Items` raccolta

Quando si associano dati al controllo Repeater, mano che passa attraverso la stessa sequenza di passaggi. L'unica differenza è che invece di `DataListItem` istanze viene create, il controllo Repeater utilizza [ `RepeaterItem` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeateritem(VS.80).aspx)s.

> [!NOTE]
> Il lettore attenti notato una lieve anomalie tra la sequenza di passaggi di quando il DataList e Repeater vengono associati a dati rispetto a quando il controllo GridView viene associato a dati. Alla fine della parte finale del processo di associazione di dati, il controllo GridView genera il `DataBound` evento; tuttavia, controllo DataList né Ripetitore dispone di un evento. In questo modo i controlli DataList e Repeater creati nuovamente nell'intervallo di tempo ASP.NET 1. x, prima che il modello di gestore di evento di pre e post-livello diventa più comune.


Come in GridView, un'opzione per la formattazione in base ai dati consiste nel creare un gestore eventi per il `ItemDataBound` evento. Questo gestore eventi potrebbe analizzare i dati che erano stati associati solo al `DataListItem` o `RepeaterItem` e influire sulla formattazione del controllo in base alle esigenze.

Per il controllo DataList, le modifiche di formattazione per l'intero elemento può essere implementata utilizzando il `DataListItem` proprietà correlate allo stile s, che includono lo standard `Font`, `ForeColor`, `BackColor`, `CssClass`e così via. Per applicare la formattazione dei controlli Web specifici all'interno del modello di DataList s, è necessario accedere a livello di codice e di modificare lo stile dei controlli Web. È stato illustrato come eseguire di nuovo nel *formattazione basato su dati personalizzati* esercitazione. Come il controllo Repeater, il `RepeaterItem` classe non dispone di alcuna proprietà correlate allo stile; pertanto, tutte le modifiche correlate allo stile apportate una `RepeaterItem` nel `ItemDataBound` gestore dell'evento deve essere eseguito da accesso a livello di codice e aggiornamento dei controlli Web all'interno di il modello.

Poiché il `ItemDataBound` formattazione tecnica per il DataList e Repeater sono quasi identici, questo esempio sarà incentrata sull'uso di DataList.

## <a name="step-1-displaying-product-information-in-the-datalist"></a>Passaggio 1: Visualizzazione di informazioni sui prodotti in DataList

Prima di preoccupazione la formattazione, s consentono di creare una pagina che utilizza un controllo DataList per visualizzare informazioni sul prodotto. Nel [esercitazione precedente](displaying-data-with-the-datalist-and-repeater-controls-cs.md) è stato creato un controllo DataList cui `ItemTemplate` visualizzato ogni nome di prodotto s, categoria, fornitore, quantità per l'unità e prezzo. Consente di ripetere qui questa funzionalità in questa esercitazione s. A tale scopo, è possibile ricreare sia DataList e ObjectDataSource da zero, oppure è possibile copiare su di essi dalla pagina creata nell'esercitazione precedente (`Basics.aspx`) e incollarli nella pagina per questa esercitazione (`Formatting.aspx`).

Una volta che si siano state replicate le funzionalità di DataList e ObjectDataSource da `Basics.aspx` in `Formatting.aspx`, è opportuno modificare DataList s `ID` proprietà `DataList1` per un nome più descrittivo `ItemDataBoundFormattingExample`. Successivamente, visualizzare DataList in un browser. Come illustrato nella figura 1, l'unica differenza di formattazione tra ogni prodotto è che il colore di sfondo alternativi.


[![Sono elencati i prodotti del controllo DataList](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image2.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image1.png)

**Figura 1**: sono elencati i prodotti di nel controllo DataList ([fare clic per visualizzare l'immagine ingrandita](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image3.png))


Per questa esercitazione, lasciare s formattare DataList in modo che tutti i prodotti con prezzo minore di $20.00 avranno entrambi il nome e prezzo unitario evidenziato in giallo.

## <a name="step-2-programmatically-determining-the-value-of-the-data-in-the-itemdatabound-event-handler"></a>Passaggio 2: Determinare a livello di codice il valore dei dati nel gestore eventi ItemDataBound

Poiché solo i prodotti con prezzo in $20.00 verrà applicata la formattazione personalizzata, è necessario essere in grado di determinare il prezzo di ogni s del prodotto. Quando si associano dati a un controllo DataList, DataList enumera i record nell'origine dati e, per ogni record, crea un `DataListItem` istanza, il record di origine dati di associazione di `DataListItem`. Dopo il record specifico s dati sono stati associati all'oggetto corrente `DataListItem` oggetto DataList s `ItemDataBound` viene generato l'evento. È possibile creare un gestore eventi per questo evento controllare i valori dei dati per l'oggetto corrente `DataListItem` e, in base alle tali valori, apportare le modifiche alla formattazione necessarie.

Creare un `ItemDataBound` evento per il controllo DataList e aggiungere il codice seguente:


[!code-csharp[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample1.cs)]

Mentre il concetto e semantica dietro DataList s `ItemDataBound` gestore dell'evento sono uguali a quelli utilizzati da s GridView `RowDataBound` nel gestore dell'evento di *formattazione basato su dati personalizzati* esercitazione, la sintassi differisce leggermente. Quando il `ItemDataBound` viene generato l'evento, il `DataListItem` solo un'associazione a dati viene passata nel gestore eventi tramite `e.Item` (anziché `e.Row`, come con i dispositivi di GridView `RowDataBound` gestore dell'evento). DataList s `ItemDataBound` gestore dell'evento viene generato per *ogni* riga aggiunta alla DataList, incluse le righe di intestazione, le righe di piè di pagina e separatore righe. Tuttavia, le informazioni sul prodotto associati solo alle righe di dati. Pertanto, quando si utilizza il `ItemDataBound` controllare i dati dell'evento associato a DataList, è necessario verificare innanzitutto che abbiamo re l'utilizzo di un elemento di dati. Questa operazione può essere eseguita controllando la `DataListItem` s [ `ItemType` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistitem.itemtype.aspx), che può avere uno dei [i seguenti valori di otto](https://msdn.microsoft.com/library/system.web.ui.webcontrols.listitemtype.aspx):

- `AlternatingItem`
- `EditItem`
- `Footer`
- `Header`
- `Item`
- `Pager`
- `SelectedItem`
- `Separator`

Entrambi `Item` e `AlternatingItem``DataListItem` gli elementi di dati di struttura DataList s s. Presupponendo che si re utilizzano un `Item` o `AlternatingItem`, effettuato l'accesso effettivo `ProductsRow` istanza associato all'oggetto corrente `DataListItem`. Il `DataListItem` s [ `DataItem` proprietà](https://msdn.microsoft.com/system.web.ui.webcontrols.datalistitem.dataitem.aspx) contiene un riferimento al `DataRowView` oggetto la cui proprietà `Row` proprietà fornisce un riferimento a effettivi `ProductsRow` oggetto.

Successivamente, controlliamo la `ProductsRow` istanza s `UnitPrice` proprietà. Poiché la tabella Products s `UnitPrice` campo consente `NULL` valori, prima di tentare di accedere il `UnitPrice` proprietà dovrebbe controllare innanzitutto la presenza di un `NULL` valore utilizzando il `IsUnitPriceNull()` metodo. Se il `UnitPrice` valore non è `NULL`, abbiamo verificare quindi se è minore di $20.00 s. Se è effettivamente in $20.00, è necessario applicare la formattazione personalizzata.

## <a name="step-3-highlighting-the-product-s-name-and-price"></a>Passaggio 3: Evidenziare il nome del prodotto s e il prezzo

Una volta che si sa che un prezzo del prodotto s è minore di $20.00, è comunque di evidenziare il nome e il prezzo. A tale scopo, è necessario innanzitutto a livello di codice fanno riferimento i controlli etichetta nel `ItemTemplate` che visualizzano il nome del prodotto s e il prezzo. Successivamente, è necessario visualizzare uno sfondo giallo. Queste informazioni di formattazione possono essere applicate modificando direttamente le etichette `BackColor` proprietà (`LabelID.BackColor = Color.Yellow`); in teoria, tuttavia, tutti gli aspetti correlati alla visualizzazione devono essere espresso tramite fogli di stile CSS. In effetti, già disponibile un foglio di stile che fornisce la formattazione desiderata definito in `Styles.css`  -  `AffordablePriceEmphasis`, che è stato creato e descritti nel *formattazione basato su dati personalizzati* esercitazione.

Per applicare la formattazione, è sufficiente impostare i due controlli etichetta Web `CssClass` proprietà `AffordablePriceEmphasis`, come illustrato nel codice seguente:


[!code-csharp[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample2.cs)]

Con il `ItemDataBound` completata dal gestore di evento, rivedere il `Formatting.aspx` pagina in un browser. Come illustrato nella figura 2, i prodotti con prezzo in $20.00 hanno nome e i relativi prezzi evidenziato.


[![Tali prodotti meno di $20.00 sono evidenziati](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image5.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image4.png)

**Figura 2**: quelli prodotti inferiore a $20.00 sono evidenziati ([fare clic per visualizzare l'immagine ingrandita](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image6.png))


> [!NOTE]
> Poiché il controllo DataList viene eseguito il rendering come HTML `<table>`, l'oggetto `DataListItem` istanze hanno proprietà correlate allo stile che è possibile impostare per applicare uno stile specifico per l'intero elemento. Ad esempio, se si desidera evidenziare il *intero* elemento giallo quando il prezzo è minore di $20.00, è stato possibile sono sostituiti il codice che fa le etichette e impostare i relativi `CssClass` proprietà con la riga di codice seguente: `e.Item.CssClass = "AffordablePriceEmphasis"` (vedere la figura 3).


Il `RepeaterItem` che costituiscono il controllo Repeater, tuttavia, t don offrono tali proprietà a livello di stile. Pertanto, l'applicazione della formattazione personalizzata per il controllo Repeater richiede che l'applicazione di proprietà di stile per i controlli Web all'interno dei modelli Ripetitore s, solo come abbiamo visto nella figura 2.


[![Per i prodotti in $20.00 viene evidenziato l'intero elemento del prodotto](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image8.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image7.png)

**Figura 3**: viene evidenziato l'intero elemento prodotto per i prodotti in $20.00 ([fare clic per visualizzare l'immagine ingrandita](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image9.png))


## <a name="using-formatting-functions-from-within-the-template"></a>Utilizzo delle funzioni di formattazione all'interno del modello

Nel *TemplateFields utilizzo nel controllo GridView* esercitazione è stato illustrato come utilizzare una funzione di formattazione in un GridView TemplateField per applicare la formattazione personalizzata in base ai dati associati alle righe s GridView. Una funzione di formattazione è un metodo che può essere richiamato da un modello e restituisce il codice HTML per essere inviati al suo posto. Formattazione di funzioni può risiedere la classe code-behind di pagine ASP.NET o può essere centralizzate in file di classe nella `App_Code` cartella o in un progetto libreria di classi separato. La funzione di formattazione classe code-behind ASP.NET pagina s lo spostamento è ideale se si prevede di utilizzare la stessa funzione di formattazione in più pagine ASP.NET o in altre applicazioni web ASP.NET.

Per illustrare le funzioni di formattazione, s consentono di disporre delle informazioni di prodotto includono il testo [DISCONTINUED] accanto al nome del prodotto s se si s non più disponibile. Consentono di s necessario, inoltre, se il giallo evidenziato prezzo è minore di $20.00 s (come nel `ItemDataBound` esempio di gestore eventi); se il prezzo è $20.00 o superiore, consentono s non visualizzare il prezzo effettivo, ma chiamare invece il testo, eseguire per un'offerta di prezzo. La figura 4 mostra una schermata dei prodotti Elenca le regole di formattazione applicate.


[![Per i prodotti dispendiosa, il prezzo viene sostituito con il testo,. chiamare per un'offerta di prezzo](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image11.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image10.png)

**Figura 4**: per i prodotti dispendiosa, il prezzo viene sostituito con il testo,. chiamare per un'offerta di prezzo ([fare clic per visualizzare l'immagine ingrandita](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image12.png))


## <a name="step-1-create-the-formatting-functions"></a>Passaggio 1: Creare le funzioni di formattazione

Per questo esempio abbiamo bisogno di due funzioni di formattazione, una che visualizza il nome del prodotto e il testo [DISCONTINUED], se necessario e un altro che consente di visualizzare entrambi un prezzo evidenziato se si s minore di $20.00 o il testo,. chiamare per un'offerta di prezzo in caso contrario. Consente di creare queste funzioni nella classe di codice della pagina s ASP.NET e assegnare loro un nome s `DisplayProductNameAndDiscontinuedStatus` e `DisplayPrice`. Entrambi i metodi devono restituire il codice HTML per eseguire il rendering sotto forma di stringa e deve essere contrassegnato sia `Protected` (o `Public`) per essere richiamati da parte di sintassi dichiarativa s pagina ASP.NET. Il codice per questi due metodi segue:


[!code-csharp[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample3.cs)]

Si noti che il `DisplayProductNameAndDiscontinuedStatus` metodo accetta i valori del `productName` e `discontinued` i campi di dati come valori scalari, mentre il `DisplayPrice` metodo accetta un `ProductsRow` istanza (anziché un `unitPrice` valore scalare). Entrambi gli approcci funzionerà; Tuttavia, funzioni con valori scalari che possono contenere i database se la funzione di formattazione `NULL` valori (ad esempio `UnitPrice`; nessuna delle due `ProductName` né `Discontinued` consentire `NULL` valori), necessario prestare particolare attenzione in la gestione di questi input scalari.

In particolare, il parametro di input deve essere di tipo `Object` poiché il valore in ingresso potrebbe essere un `DBNull` istanza anziché il tipo di dati previsto. Inoltre, deve essere eseguito un controllo per determinare se il valore in ingresso è un database `NULL` valore. Ovvero, se si voleva il `DisplayPrice` metodo per accettare il prezzo come un valore scalare, d è necessario utilizzare il codice seguente:


[!code-csharp[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample4.cs)]

Si noti che il `unitPrice` parametro di input è di tipo `Object` e che l'istruzione condizionale è stata modificata per verificare se `unitPrice` è `DBNull` o non. Inoltre, poiché il `unitPrice` parametro di input viene passato come un `Object`, è necessario eseguire il cast su un valore decimale.

## <a name="step-2-calling-the-formatting-function-from-the-datalist-s-itemtemplate"></a>Passaggio 2: Chiamata la funzione di formattazione da ItemTemplate DataList s

Con le funzioni di formattazione aggiunte per la classe code-behind di pagine ASP.NET, è comunque per richiamare tali funzioni da DataList s di formattazione `ItemTemplate`. Per chiamare una funzione di formattazione da un modello, effettuare la chiamata di funzione all'interno di sintassi di associazione dati:


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample5.aspx)]

In DataList s `ItemTemplate` il `ProductNameLabel` controllo etichetta Web attualmente vengono visualizzati il nome del prodotto s assegnando il relativo `Text` proprietà il risultato di `<%# Eval("ProductName") %>`. Per avere Visualizza il nome e il testo [DISCONTINUED], se necessario, aggiornare la sintassi dichiarativa in modo che invece assegna il `Text` il valore della proprietà del `DisplayProductNameAndDiscontinuedStatus` metodo. In tal caso, è necessario passare il nome del prodotto s e i valori obsoleti utilizzando il `Eval("columnName")` sintassi. `Eval` Restituisce un valore di tipo `Object`, ma la `DisplayProductNameAndDiscontinuedStatus` metodo prevede parametri di input di tipo `String` e `Boolean`; pertanto, è necessario eseguire il cast di valori restituiti dal `Eval` metodo ai tipi di parametro di input previsto, come illustrato di seguito:


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample6.aspx)]

Per visualizzare il prezzo, è possibile impostare semplicemente il `UnitPriceLabel` etichetta s `Text` il valore restituito dalla proprietà di `DisplayPrice` metodo, esattamente come è stato fatto per la visualizzazione del nome di prodotto s e [SOSPESI] testo. Tuttavia, anziché passare il `UnitPrice` come un parametro di input scalare, viene invece passato nell'intero `ProductsRow` istanza:


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample7.aspx)]

Con le chiamate alle funzioni di formattazione sul posto, richiedere qualche istante per visualizzare l'avanzamento in un browser. La schermata dovrebbe essere simile alla figura 5, con i prodotti non più supportati, incluso il testo [DISCONTINUED] e i prodotti con costo più di $20.00 con i relativi prezzi sostituito con il testo, chiamata di un'offerta di prezzo.


[![Per i prodotti dispendiosa, il prezzo viene sostituito con il testo,. chiamare per un'offerta di prezzo](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image14.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image13.png)

**Figura 5**: per i prodotti dispendiosa, il prezzo viene sostituito con il testo,. chiamare per un'offerta di prezzo ([fare clic per visualizzare l'immagine ingrandita](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image15.png))


## <a name="summary"></a>Riepilogo

Formattazione del contenuto di un controllo DataList o Repeater in base ai dati può essere eseguita utilizzando due tecniche. La prima tecnica consiste nel creare un gestore eventi per il `ItemDataBound` evento, che viene generato come ogni record nell'origine dati è associato a un nuovo `DataListItem` o `RepeaterItem`. Nel `ItemDataBound` gestore eventi, i dati dell'elemento s corrente possono essere esaminati e quindi può essere applicata la formattazione per il contenuto del modello o per `DataListItem` s, per l'intero elemento stesso.

In alternativa, la formattazione personalizzata può essere realizzata tramite le funzioni di formattazione. Una funzione di formattazione è un metodo che può essere richiamato da DataList Ripetitore s modelli o che restituisce il codice HTML da creare al suo posto. Spesso, il codice HTML restituito da una funzione di formattazione è determinato dai valori da associare all'elemento corrente. Questi valori possono essere passati alla funzione di formattazione, come valori scalari o passando l'intero oggetto da associare all'elemento (ad esempio il `ProductsRow` istanza).

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Lead revisori per questa esercitazione sono stati Yaakov Ellis Randy Schmidt e Liz Shulok. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](displaying-data-with-the-datalist-and-repeater-controls-cs.md)
> [Successivo](showing-multiple-records-per-row-with-the-datalist-control-cs.md)
