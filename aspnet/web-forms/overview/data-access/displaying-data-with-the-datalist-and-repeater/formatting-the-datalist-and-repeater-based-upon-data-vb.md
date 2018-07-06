---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-vb
title: Formattazione di DataList e Repeater in base ai dati (VB) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione verrà esaminare esempi di come si formatta l'aspetto dei controlli DataList e Repeater, tramite l'utilizzo di funzioni di formattazione con...
ms.author: aspnetcontent
ms.date: 09/13/2006
ms.assetid: e2f401ae-37bb-4b19-aa97-d6b385d40f88
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 428438b2bae062c09d13c002f4729c3c394975a1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807710"
---
<a name="formatting-the-datalist-and-repeater-based-upon-data-vb"></a>Formattazione di DataList e Repeater in base ai dati (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'App di esempio](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_30_VB.exe) o [Scarica il PDF](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/datatutorial30vb1.pdf)

> In questa esercitazione verrà esaminare esempi di come si formatta l'aspetto dei controlli DataList e Repeater, utilizzando le funzioni di formattazione all'interno dei modelli o gestendo l'evento associato a dati.


## <a name="introduction"></a>Introduzione

Come illustrato nell'esercitazione precedente, DataList offre una serie di proprietà correlate allo stile che interessano l'aspetto del controllo. In particolare, abbiamo visto come assegnare predefinito classi CSS a s DataList `HeaderStyle`, `ItemStyle`, `AlternatingItemStyle`, e `SelectedItemStyle` proprietà. Oltre a queste quattro proprietà DataList include una serie di altre proprietà correlate allo stile, ad esempio `Font`, `ForeColor`, `BackColor`, e `BorderWidth`, per citarne alcuni. Non contiene le proprietà correlate allo stile del controllo Repeater. Direttamente all'interno del markup nei modelli s Repeater, è necessario configurare tali impostazioni di stile.

Spesso, tuttavia, la formattazione dei dati dipende dai dati stesso. Ad esempio, quando si elencano i prodotti potrebbe essere necessario visualizzare le informazioni sul prodotto in un colore del tipo di carattere grigio chiaro se si non è più disponibile oppure si può decidere di evidenziare il `UnitsInStock` valore se è zero. Come abbiamo visto nelle esercitazioni precedenti, il controllo GridView, DetailsView e FormView offre due modi diversi per formattare i dati in base a come appaiono:

- **Il `DataBound` evento** creare un gestore eventi per l'oggetto appropriato `DataBound` evento, che viene attivato dopo che i dati sono stati associati a ogni elemento (per il controllo GridView è stato il `RowDataBound` eventi; per DataList e Repeater è la `ItemDataBound`evento). In questo caso può essere esaminati gestore, solo i dati associati e formattazione decisioni impostata. Abbiamo esaminato questa tecnica nel [formattazione basata su dati personalizzati](../custom-formatting/custom-formatting-based-upon-data-vb.md) esercitazione.
- **Le funzioni nei modelli di formattazione** quando uso di TemplateFields nei controlli DetailsView o GridView, o un modello nel controllo FormView, possiamo aggiungere una funzione di formattazione per la classe code-behind s pagina ASP.NET, il livello di logica di Business o i altra libreria di classi che è accessibile dall'applicazione web. Questa funzione di formattazione può accettare un numero arbitrario di parametri di input, ma deve restituire il codice HTML per eseguire il rendering nel modello. Funzioni di formattazione prima di tutto sono state esaminate nel [uso di TemplateFields nel controllo GridView](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) esercitazione.

Entrambe le tecniche di formattazione disponibili con i controlli DataList e Repeater. In questa esercitazione verrà esaminare esempi che usano entrambe le tecniche per entrambi i controlli.

## <a name="using-theitemdataboundevent-handler"></a>Uso di`ItemDataBound`gestore dell'evento

Quando i dati sono associati a un controllo DataList, da un controllo origine dati o tramite l'assegnazione a livello di programmazione di dati al controllo s `DataSource` proprietà e chiamare il metodo relativo `DataBind()` metodo, il controllo DataList s `DataBinding` evento viene generato, l'origine di dati enumerato, e ogni record di dati è associato a DataList. Per ogni record nell'origine dati, DataList crea una [ `DataListItem` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistitem.aspx) oggetto quindi associato al record corrente. Durante questo processo, DataList genera due eventi:

- **`ItemCreated`** viene attivato dopo il `DataListItem` è stato creato
- **`ItemDataBound`** viene attivato dopo che il record corrente è stato associato per il `DataListItem`

I passaggi seguenti illustrano il processo di associazione dati per il controllo DataList.

1. S DataList [ `DataBinding` evento](https://msdn.microsoft.com/library/system.web.ui.control.databinding.aspx) viene attivato
2. I dati vengono associati a DataList  
  
   Per ogni record nell'origine dati 

    1. Creare un `DataListItem` oggetto
    2. Attivare i [ `ItemCreated` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.itemcreated.aspx)
    3. Associare il record per il `DataListItem`
    4. Attivare i [ `ItemDataBound` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.itemdatabound.aspx)
    5. Aggiungere il `DataListItem` per il `Items` raccolta

Quando si associano dati al controllo Repeater, il relativo avanzamento attraverso la stessa identica sequenza di passaggi. L'unica differenza è che invece `DataListItem` istanze viene create, utilizza il controllo Repeater [ `RepeaterItem` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeateritem(VS.80).aspx)s.

> [!NOTE]
> Il lettore astuto potrebbe aver notato una lieve anomalie tra la sequenza di passaggi che passano quando DataList e Repeater vengono associati a dati rispetto a quando il controllo GridView è associato a dati. Nella parte finale del processo di associazione dati, genera il controllo GridView di `DataBound` evento; tuttavia, nessuna delle due DataList e Repeater controllo dispone di tale evento. Questo avviene perché i controlli DataList e Repeater sono stati creati nuovamente nell'intervallo di tempo ASP.NET 1.x, prima che il criterio del gestore di evento di pre-elaborazione e post-livello diventa più comune.


Come in GridView, una delle opzioni per la formattazione in base ai dati consiste nel creare un gestore eventi per il `ItemDataBound` evento. Questo gestore eventi verrebbe controllare i dati che sono appena stati associati ai `DataListItem` o `RepeaterItem` e cambia la formattazione del controllo in base alle esigenze.

Per il controllo DataList, le modifiche di formattazione per l'intero elemento può essere implementata usando il `DataListItem` proprietà correlate allo stile s, che includono lo standard `Font`, `ForeColor`, `BackColor`, `CssClass`e così via. Per applicare la formattazione dei controlli Web specifici all'interno del modello s DataList, dobbiamo accedere e modificare lo stile dei controlli Web a livello di codice. Abbiamo visto come eseguire questa back nel *formattazione basata su dati personalizzati* esercitazione. Come il controllo Repeater, il `RepeaterItem` classe dispone di alcuna proprietà correlate allo stile; pertanto, tutte le modifiche correlate allo stile apportate a un `RepeaterItem` nel `ItemDataBound` gestore dell'evento deve essere eseguito dall'accesso a livello di codice e aggiornamento dei controlli Web all'interno di il modello.

Poiché il `ItemDataBound` tecnica di formattazione di DataList e Repeater sono praticamente identici, questo esempio verrà posto sull'uso di DataList.

## <a name="step-1-displaying-product-information-in-the-datalist"></a>Passaggio 1: Visualizzare le informazioni sul prodotto in DataList

Prima ci preoccupiamo la formattazione, s ti permettono di creare una pagina che usa un controllo DataList per visualizzare le informazioni sul prodotto. Nel [esercitazione precedente](displaying-data-with-the-datalist-and-repeater-controls-vb.md) abbiamo creato un controllo DataList di cui `ItemTemplate` visualizzato ogni nome di prodotto s, categoria, fornitore, quantity per ogni unità e il prezzo. Let s ripetere qui questa funzionalità in questa esercitazione. A tale scopo, è possibile ricreare sia DataList e relativo ObjectDataSource da zero, oppure è possibile copiare su di essi dalla pagina creata nell'esercitazione precedente (`Basics.aspx`) e li incollo nella pagina per questa esercitazione (`Formatting.aspx`).

Una volta che si siano state replicate le funzionalità di DataList e ObjectDataSource dalla `Basics.aspx` nelle `Formatting.aspx`, si consiglia di modifica di DataList s `ID` proprietà dal `DataList1` a un nome più descrittivo `ItemDataBoundFormattingExample`. Successivamente, visualizzare il controllo DataList in un browser. Come illustrato nella figura 1, l'unica differenza formattazione tra ogni prodotto è che alterna il colore di sfondo.


[![I prodotti sono elencati nel controllo DataList](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image2.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image1.png)

**Figura 1**: The prodotti sono elencati nel controllo DataList ([fare clic per visualizzare l'immagine con dimensioni normali](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image3.png))


Per questa esercitazione, lasciare s formattazione di DataList tale che tutti i prodotti con prezzo minore di 20,00 dollari avrà sia il nome e unit price evidenziato in giallo.

## <a name="step-2-programmatically-determining-the-value-of-the-data-in-the-itemdatabound-event-handler"></a>Passaggio 2: Determinare a livello di codice il valore dei dati nel gestore dell'evento ItemDataBound

Avendo solo i prodotti con prezzo in 20,00 dollari verrà applicata la formattazione personalizzata, è necessario essere in grado di determinare ogni prezzo per i prodotti. Quando si associano dati a un controllo DataList, DataList enumera i record nell'origine dati e, per ogni record, viene creato un `DataListItem` istanza, il record di origine dati di associazione di `DataListItem`. Dopo il record specifico s dei dati sono stati associati all'oggetto corrente `DataListItem` oggetti, DataList s `ItemDataBound` evento. È possibile creare un gestore eventi per questo evento per controllare i valori dei dati per l'oggetto corrente `DataListItem` e, in base al tali valori, apportare le modifiche alla formattazione necessarie.

Creare un `ItemDataBound` evento per il controllo DataList e aggiungere il codice seguente:


[!code-vb[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample1.vb)]

Mentre i concetti e la semantica dietro s DataList `ItemDataBound` gestore dell'evento sono uguali a quelle utilizzate dalla s GridView `RowDataBound` gestore dell'evento nel *formattazione basata su dati personalizzati* esercitazione, la sintassi differisce leggermente. Quando la `ItemDataBound` viene generato l'evento, il `DataListItem` semplicemente un'associazione a dati viene passata nel gestore eventi corrispondente tramite `e.Item` (anziché `e.Row`, come con i dispositivi di GridView `RowDataBound` gestore dell'evento). S DataList `ItemDataBound` gestore dell'evento viene generato per *ogni* riga aggiunta alla DataList, tra cui le righe di intestazione, piè di pagina righe e righe di separazione. Tuttavia, le informazioni sul prodotto è associati solo alle righe di dati. Pertanto, quando si usa il `ItemDataBound` evento per controllare i dati associati a DataList, è necessario innanzitutto verificare che si ri utilizzo di un elemento di dati. Questa operazione può essere eseguita controllando la `DataListItem` s [ `ItemType` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistitem.itemtype.aspx), che può avere uno dei [i valori seguenti otto](https://msdn.microsoft.com/library/system.web.ui.webcontrols.listitemtype.aspx):

- `AlternatingItem`
- `EditItem`
- `Footer`
- `Header`
- `Item`
- `Pager`
- `SelectedItem`
- `Separator`

Entrambe `Item` e `AlternatingItem``DataListItem` gli elementi di dati di s trucco s DataList. Supponendo che si ri funziona con un `Item` o `AlternatingItem`, è possibile accedere effettivo `ProductsRow` istanza in cui è stata associata all'oggetto corrente `DataListItem`. Il `DataListItem` s [ `DataItem` proprietà](https://msdn.microsoft.com/system.web.ui.webcontrols.datalistitem.dataitem.aspx) contiene un riferimento al `DataRowView` dell'oggetto, la cui proprietà `Row` proprietà fornisce un riferimento all'effettivo `ProductsRow` oggetto.

Successivamente, controlliamo la `ProductsRow` istanza s `UnitPrice` proprietà. Poiché la tabella Products s `UnitPrice` campo consente `NULL` valori, prima di tentare di accedere il `UnitPrice` proprietà dovrebbe controllare innanzitutto la presenza di un `NULL` valore utilizzando il `IsUnitPriceNull()` (metodo). Se il `UnitPrice` il valore non è `NULL`, viene quindi verificato se è minore rispetto a 20,00 dollari. Se è effettivamente in 20,00 dollari, è quindi necessario applicare la formattazione personalizzata.

## <a name="step-3-highlighting-the-product-s-name-and-price"></a>Passaggio 3: Evidenzia le s nome prodotto e prezzo

Una volta sappiamo che un prezzo del prodotto s è minore di 20,00 dollari, che resta evidenziare il nome e il prezzo. A tale scopo, è necessario prima a livello di programmazione di fare riferimento ai controlli Label nel `ItemTemplate` che visualizzano il nome del prodotto s e prezzo. Successivamente, è necessario in modo che vengano visualizzati uno sfondo giallo. Queste informazioni di formattazione possono essere applicate direttamente modificando le etichette `BackColor` delle proprietà (`LabelID.BackColor = Color.Yellow`); in teoria, tuttavia, tutti gli aspetti correlati alla visualizzazione devono essere espresso tramite fogli di stile CSS. Infatti, si dispone già di un foglio di stile che fornisce la formattazione desiderata definito in `Styles.css`  -  `AffordablePriceEmphasis`, che è stato creato e illustrate nel *formattazione basata su dati personalizzati* esercitazione.

Per applicare la formattazione, è sufficiente impostare i due controlli Web Label `CssClass` proprietà `AffordablePriceEmphasis`, come illustrato nel codice seguente:


[!code-vb[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample2.vb)]

Con il `ItemDataBound` completata dal gestore di eventi, visitare di nuovo il `Formatting.aspx` pagina in un browser. Come illustrato nella figura 2, i prodotti con prezzo in 20,00 dollari hanno sia il nome e il prezzo evidenziato.


[![Tali prodotti minore 20,00 dollari vengono evidenziati](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image5.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image4.png)

**Figura 2**: vengono evidenziati i prodotti minore 20,00 dollari ([fare clic per visualizzare l'immagine con dimensioni normali](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image6.png))


> [!NOTE]
> Poiché il controllo DataList viene visualizzato come un elemento HTML `<table>`, la relativa `DataListItem` istanze hanno proprietà correlate allo stile che possono essere impostate per applicare uno stile specifico per l'intero elemento. Ad esempio, se si desidera evidenziare il *intera* elemento giallo quando essa il prezzo è minore di 20,00 dollari, è stato possibile hanno sostituito il codice a cui fa riferimento le etichette e impostare loro `CssClass` delle proprietà con la riga di codice seguente: `e.Item.CssClass = "AffordablePriceEmphasis"` (vedere la figura 3).


Il `RepeaterItem` che costituiscono il controllo Repeater, tuttavia, don t offrono tali proprietà a livello di stile. Pertanto, applicando la formattazione personalizzata per il controllo Repeater richiede che l'applicazione di proprietà di stile ai controlli Web all'interno dei modelli di Repeater s, come abbiamo fatto nella figura 2.


[![Viene evidenziato l'intero elemento prodotto per i prodotti in 20,00 dollari](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image8.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image7.png)

**Figura 3**: viene evidenziato l'intero elemento prodotto per i prodotti in 20,00 dollari ([fare clic per visualizzare l'immagine con dimensioni normali](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image9.png))


## <a name="using-formatting-functions-from-within-the-template"></a>Utilizzo delle funzioni di formattazione all'interno del modello

Nel *uso di TemplateFields nel controllo GridView* esercitazione è stato illustrato come utilizzare una funzione di formattazione all'interno di un GridView TemplateField per applicare la formattazione personalizzata in base ai dati associati a righe s GridView. Una funzione di formattazione è un metodo che può essere richiamato da un modello e restituisce il codice HTML per essere inviati al suo posto. Funzioni di formattazione possono risiedere nella classe code-behind ASP.NET pagina s o possono essere centralizzate in file di classe nel `App_Code` cartella o in un progetto libreria di classi separato. Spostamento della funzione di formattazione dalla classe code-behind s pagina ASP.NET è ideale se si prevede di usare la stessa funzione di formattazione in più pagine ASP.NET o in altre applicazioni web ASP.NET.

Per illustrare le funzioni di formattazione, ti permettono di s sono le informazioni sul prodotto includere il testo [DISCONTINUED] accanto al nome del prodotto s se si s non più disponibile. Inoltre, ti permettono di s hanno if giallo evidenziato prezzo s minore 20,00 dollari (avuti `ItemDataBound` esempio di gestore eventi); se il prezzo è 20,00 dollari o versione successiva, "Let" s non visualizzano il prezzo effettivo, ma chiamare invece il testo, Please per un'offerta di prezzo. Figura 4 mostra una schermata dei prodotti listato con queste regole di formattazione applicate.


[![Per i prodotti costosa, il prezzo viene sostituito con il testo,. chiamare per un preventivo](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image11.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image10.png)

**Figura 4**: per Expensive Products, il prezzo viene sostituito con il testo,. chiamare per un preventivo ([fare clic per visualizzare l'immagine con dimensioni normali](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image12.png))


## <a name="step-1-create-the-formatting-functions"></a>Passaggio 1: Creare le funzioni di formattazione

Per questo esempio sono necessari due funzioni di formattazione, consente di visualizzare il nome del prodotto insieme al testo [DISCONTINUED], se necessario e un altro che consente di visualizzare entrambi un prezzo evidenziato se si s minore 20,00 dollari o il testo,. chiamare per un'offerta di prezzo in caso contrario. Let s creare queste funzioni nella classe code-behind ASP.NET pagina s e denominarli `DisplayProductNameAndDiscontinuedStatus` e `DisplayPrice`. Entrambi i metodi devono restituire il codice HTML per eseguire il rendering sotto forma di stringa ed entrambi devono essere contrassegnati `Protected` (o `Public`) per essere richiamati dalla parte di sintassi dichiarativa per pagina s di ASP.NET. Il codice per questi due metodi segue:


[!code-vb[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample3.vb)]

Si noti che il `DisplayProductNameAndDiscontinuedStatus` metodo accetta i valori del `productName` e `discontinued` data i campi come valori scalari, mentre il `DisplayPrice` metodo accetta una `ProductsRow` istanza (anziché un `unitPrice` valore scalare). Entrambi gli approcci funzionerà; Tuttavia, funzioni con valori scalari che possono contenere database se la funzione di formattazione `NULL` valori (, ad esempio `UnitPrice`; nessuno dei due `ProductName` né `Discontinued` consentire `NULL` valori), è necessario prestare particolare attenzione nella gestione di questi input scalari.

In particolare, il parametro di input deve essere di tipo `Object` poiché il valore in ingresso potrebbe essere una `DBNull` istanza anziché il tipo di dati previsto. Inoltre, deve essere eseguito un controllo per determinare se il valore in ingresso è un database `NULL` valore. Vale a dire, se volessimo il `DisplayPrice` metodo per accettare il prezzo come un valore scalare, è il d è necessario usare il codice seguente:


[!code-vb[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample4.vb)]

Si noti che il `unitPrice` parametro di input è di tipo `Object` e che l'istruzione condizionale è stato modificato per accertare se `unitPrice` è `DBNull` oppure No. Inoltre, poiché il `unitPrice` parametro di input viene passato come un `Object`, è necessario eseguirne il cast su un valore decimale.

## <a name="step-2-calling-the-formatting-function-from-the-datalist-s-itemtemplate"></a>Passaggio 2: Chiamando la funzione di formattazione di DataList s ItemTemplate

Con le funzioni di formattazione aggiunte alla classe code-behind ASP.NET pagina s, tutto resta che chiamare queste funzioni da DataList s di formattazione `ItemTemplate`. Per chiamare una funzione di formattazione da un modello, inserire la chiamata di funzione all'interno di sintassi di associazione dati:


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample5.aspx)]

In s DataList `ItemTemplate` il `ProductNameLabel` controllo etichetta Web attualmente visualizzato il nome del prodotto s assegnando relativo `Text` proprietà il risultato di `<%# Eval("ProductName") %>`. Per fare in modo visualizzi il nome e il testo [DISCONTINUED], se necessario, aggiornare la sintassi dichiarativa in modo che l'assegnazione automatica invece i `Text` il valore della proprietà del `DisplayProductNameAndDiscontinuedStatus` (metodo). In tal caso, è necessario passare il nome del prodotto s e non più disponibili i valori mediante il `Eval("columnName")` sintassi. `Eval` Restituisce un valore di tipo `Object`, ma la `DisplayProductNameAndDiscontinuedStatus` metodo prevede parametri di input di tipo `String` e `Boolean`; pertanto, è necessario eseguire il cast di valori restituiti dai `Eval` metodo ai tipi di parametro di input previsto, come illustrato di seguito:


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample6.aspx)]

Per visualizzare il prezzo, è possibile impostare semplicemente la `UnitPriceLabel` etichetta s `Text` sul valore restituito dal `DisplayPrice` metodo, esattamente come è stato fatto per la visualizzazione dei nomi di prodotto s e [sospeso] testo. Tuttavia, anziché passare il `UnitPrice` come un parametro di input scalare, passiamo invece dell'intero `ProductsRow` istanza:


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample7.aspx)]

Con le chiamate alle funzioni di formattazione posto, si consiglia di visualizzare lo stato di avanzamento in un browser. La schermata dovrebbe essere simile alla figura 5, con i prodotti non più supportati, incluso il testo [DISCONTINUED] e i prodotti più 20,00 dollari con rispettivo prezzo di determinazione costi sostituito con il testo, chiamata di un'offerta di prezzo.


[![Per i prodotti costosa, il prezzo viene sostituito con il testo,. chiamare per un preventivo](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image14.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image13.png)

**Figura 5**: per Expensive Products, il prezzo viene sostituito con il testo,. chiamare per un preventivo ([fare clic per visualizzare l'immagine con dimensioni normali](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image15.png))


## <a name="summary"></a>Riepilogo

Formattazione del contenuto di un controllo DataList o Repeater in base ai dati è possibile usare due tecniche. La prima tecnica consiste nel creare un gestore eventi per il `ItemDataBound` evento, che viene generato in ogni record nell'origine dati è associato a una nuova `DataListItem` o `RepeaterItem`. Nel `ItemDataBound` gestore eventi, i dati dell'elemento s corrente possono essere esaminati e quindi la formattazione può essere applicata in base al contenuto del modello oppure, per `DataListItem` s, per l'intero elemento stesso.

In alternativa, la formattazione personalizzata può essere realizzata attraverso funzioni di formattazione. Una funzione di formattazione è un metodo che può essere richiamato da DataList o Repeater i modelli di s che restituisce il codice HTML a generare al suo posto. Spesso, il codice HTML restituiti da una funzione di formattazione è determinato dai valori da associare all'elemento corrente. Questi valori possono essere passati alla funzione di formattazione, come valori scalari o passando l'intero oggetto a cui è associata all'elemento (ad esempio il `ProductsRow` istanza).

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. I revisori per questa esercitazione sono Liz Shulok Yaakov Ellis e Randy Schmidt. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](displaying-data-with-the-datalist-and-repeater-controls-vb.md)
> [Successivo](showing-multiple-records-per-row-with-the-datalist-control-vb.md)
