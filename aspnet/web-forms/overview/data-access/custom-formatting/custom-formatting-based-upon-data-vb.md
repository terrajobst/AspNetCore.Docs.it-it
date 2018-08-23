---
uid: web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-vb
title: Formattazione personalizzata in base ai dati (VB) | Microsoft Docs
author: rick-anderson
description: In diversi modi, è possibile modificare il formato del controllo GridView, DetailsView o FormView in base ai dati a esso associati. In questa esercitazione, verrà l...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: df5a1525-386f-4632-972c-57b199870bc3
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-vb
msc.type: authoredcontent
ms.openlocfilehash: d902fd6d042783c036bb42a11b7e469f6dd2b5b6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832074"
---
<a name="custom-formatting-based-upon-data-vb"></a>Formattazione personalizzata in base ai dati (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'App di esempio](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_11_VB.exe) o [Scarica il PDF](custom-formatting-based-upon-data-vb/_static/datatutorial11vb1.pdf)

> In diversi modi, è possibile modificare il formato del controllo GridView, DetailsView o FormView in base ai dati a esso associati. In questa esercitazione esamineremo come eseguire l'operazione di formattazione di dati associati tramite l'utilizzo di gestori di eventi con associazione a dati e RowDataBound.


## <a name="introduction"></a>Introduzione

L'aspetto dei controlli GridView, DetailsView e FormView può essere personalizzato tramite una varietà di proprietà correlate allo stile. Le proprietà come `CssClass`, `Font`, `BorderWidth`, `BorderStyle`, `BorderColor`, `Width`, e `Height`, ad esempio, determinare l'aspetto generale del controllo viene eseguito il rendering. Le proprietà incluse `HeaderStyle`, `RowStyle`, `AlternatingRowStyle`, e altre consentono le stesse impostazioni di stile da applicare a sezioni specifiche. Allo stesso modo, queste impostazioni di stile possono essere applicate a livello di campo.

In molti scenari, tuttavia, i requisiti di formattazione dipendono dal valore dei dati visualizzati. Per attirare l'attenzione per fuori azionari prodotti, ad esempio, un report che elenca le informazioni sul prodotto potrebbe impostare il colore di sfondo su giallo per tali prodotti la cui proprietà `UnitsInStock` e `UnitsOnOrder` campi sono entrambi uguali a 0. Per evidenziare i prodotti più costosi, potremmo voler visualizzare i prezzi di questi prodotti più 75,00 in grassetto un calcolo dei costi.

In diversi modi, è possibile modificare il formato del controllo GridView, DetailsView o FormView in base ai dati a esso associati. In questa esercitazione esamineremo come eseguire l'operazione di formattazione di dati associati mediante l'utilizzo dei `DataBound` e `RowDataBound` gestori eventi. Nella prossima esercitazione verrà illustrato un approccio alternativo.

## <a name="using-the-detailsview-controlsdataboundevent-handler"></a>Uso del controllo DetailsView`DataBound`gestore dell'evento

Quando i dati sono associati a un controllo DetailsView, da un controllo origine dati o tramite l'assegnazione a livello di programmazione di dati del controllo `DataSource` proprietà e chiamare il metodo relativo `DataBind()` si verificano di metodo, la sequenza di passaggi seguente:

1. I dati controllo Web `DataBinding` viene generato l'evento.
2. I dati vengono associati ai dati di controllo Web.
3. I dati controllo Web `DataBound` viene generato l'evento.

La logica personalizzata può essere inserita immediatamente dopo i passaggi 1 e 3 tramite un gestore eventi. Tramite la creazione di un gestore eventi per il `DataBound` eventi è possibile determinare a livello di codice i dati che sono stata associata al controllo Web i dati e configurare la formattazione in base alle esigenze. Per illustrare questo scenario è possibile creare un controllo DetailsView che elenca le informazioni generali su un prodotto, ma verrà visualizzato il `UnitPrice` valore in un ***tipo di carattere grassetto, corsivo*** se supera i $75,00.

## <a name="step-1-displaying-the-product-information-in-a-detailsview"></a>Passaggio 1: Visualizzare le informazioni sul prodotto in un controllo DetailsView

Aprire il `CustomColors.aspx` nella pagina la `CustomFormatting` cartella, trascinare un controllo DetailsView dalla casella degli strumenti nella finestra di progettazione, impostare relativo `ID` valore della proprietà da `ExpensiveProductsPriceInBoldItalic`e associarlo a un nuovo controllo ObjectDataSource che richiama la `ProductsBLL` della classe `GetProducts()` (metodo). I passaggi dettagliati per questa operazione vengono omessi qui per motivi di brevità, poiché li abbiamo esaminato in dettaglio nelle esercitazioni precedenti.

Dopo che è stato associato a ObjectDataSource a DetailsView, si consiglia di modificare l'elenco dei campi. Ho scelto di rimuovere il `ProductID`, `SupplierID`, `CategoryID`, `UnitsInStock`, `UnitsOnOrder`, `ReorderLevel`, e `Discontinued` BoundField e rinominato e riformattato restanti BoundField. Inoltre eliminate le `Width` e `Height` impostazioni. Poiché il controllo DetailsView visualizza solo un singolo record, è necessario attivare il paging per consentire all'utente finale visualizzare tutti i prodotti. Eseguire questa operazione selezionando la casella di controllo Attiva Paging nello smart tag di DetailsView.


[![Figura 1: Controllare la casella di controllo Attiva Paging nello Smart Tag di DetailsView](custom-formatting-based-upon-data-vb/_static/image2.png)](custom-formatting-based-upon-data-vb/_static/image1.png)

**Figura 1**: nella figura 1: la casella Attiva Paging nello Smart Tag di DetailsView ([fare clic per visualizzare l'immagine con dimensioni normali](custom-formatting-based-upon-data-vb/_static/image3.png))


Dopo tali modifiche, il tag di DetailsView sarà:


[!code-aspx[Main](custom-formatting-based-upon-data-vb/samples/sample1.aspx)]

Si consiglia di testare questa pagina nel browser.


[![Il controllo DetailsView visualizza un solo prodotto alla volta](custom-formatting-based-upon-data-vb/_static/image5.png)](custom-formatting-based-upon-data-vb/_static/image4.png)

**Figura 2**: il controllo DetailsView controllo consente di visualizzare un prodotto alla volta ([fare clic per visualizzare l'immagine con dimensioni normali](custom-formatting-based-upon-data-vb/_static/image6.png))


## <a name="step-2-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a>Passaggio 2: Determinare a livello di codice il valore dei dati nel gestore eventi associato a dati

Per visualizzare il prezzo di un tipo di carattere grassetto, corsivo per i prodotti il cui `UnitPrice` valore superiore a $75,00, dobbiamo essere in grado di determinare a livello di codice prima di `UnitPrice` valore. Per il controllo DetailsView, ciò può essere eseguita nel `DataBound` gestore dell'evento. Per creare l'evento gestore fare clic sul controllo DetailsView nella finestra di progettazione, quindi passare alla finestra Proprietà. Premere F4 per riavviarla, se non è visibile o passare al menu di visualizzazione e selezionare l'opzione di menu Finestra proprietà. Dalla finestra delle proprietà, fare clic sull'icona a forma di fulmine per elencare gli eventi di DetailsView. Successivamente, fare doppio clic su di `DataBound` evento o digitare il nome che si desidera creare il gestore dell'evento.


![Creare un gestore eventi per l'evento associato a dati](custom-formatting-based-upon-data-vb/_static/image7.png)

**Figura 3**: creare un gestore eventi per il `DataBound` evento


> [!NOTE]
> È anche possibile creare un gestore eventi da parte di codice della pagina ASP.NET. Vi sono disponibili due elenchi a discesa nella parte superiore della pagina. Selezionare l'oggetto nell'elenco di riepilogo a discesa a sinistra e l'evento che si desidera creare un gestore per dall'elenco a discesa a destra e Visual Studio creerà automaticamente il gestore eventi appropriato.


In questo modo verrà automaticamente creato il gestore dell'evento e consente di passare alla parte di codice in cui è stato aggiunto. A questo punto verrà visualizzato:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample2.vb)]

I dati associati a DetailsView sono accessibili tramite il `DataItem` proprietà. È importante ricordare che viene eseguita l'associazione i controlli a una DataTable fortemente tipizzato, composto da una raccolta di istanze di DataRow fortemente tipizzato. Quando l'oggetto DataTable è associata a DetailsView, il primo DataRow in DataTable viene assegnato a DetailsView `DataItem` proprietà. In particolare, il `DataItem` proprietà viene assegnato un `DataRowView` oggetto. È possibile usare la `DataRowView`del `Row` proprietà per ottenere l'accesso all'oggetto DataRow sottostante, che è effettivamente un `ProductsRow` istanza. Dopo aver ottenuto questo `ProductsRow` istanza faciliteremo la nostra decisione esaminando semplicemente i valori delle proprietà dell'oggetto.

Il codice seguente illustra come determinare se il `UnitPrice` associata al controllo DetailsView è superiore a $75,00:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample3.vb)]

> [!NOTE]
> Poiché `UnitPrice` può avere un `NULL` valore nel database, controllare innanzitutto assicurarsi che Microsoft non ha a che fare con un `NULL` valore prima dell'accesso il `ProductsRow`del `UnitPrice` proprietà. Questo controllo è importante perché se si tenta di accedere al `UnitPrice` proprietà quando ha un `NULL` valore il `ProductsRow` oggetto genererà una [eccezione StrongTypingException](https://msdn.microsoft.com/library/system.data.strongtypingexception.aspx).


## <a name="step-3-formatting-the-unitprice-value-in-the-detailsview"></a>Passaggio 3: Formattazione del valore di UnitPrice in DetailsView

A questo punto è possibile determinare se il `UnitPrice` associato a DetailsView ha un valore superiore a $75,00, ma è stata ancora per vedere come modificare a livello di programmazione DetailsView di formattazione di conseguenza. Per modificare la formattazione di un'intera riga in DetailsView, accedere a livello di codice usando la riga `DetailsViewID.Rows(index)`; per modificare una cella specifica, usare l'accesso `DetailsViewID.Rows(index).Cells(index)`. Dopo aver ottenuto un riferimento alla riga o cella è quindi possibile modificare l'aspetto del controllo impostandone le proprietà correlate allo stile.

L'accesso a una riga a livello di codice, è necessario conoscere indice della riga, che inizia da 0. Il `UnitPrice` riga è la quinta riga nel controllo DetailsView, assegnandogli un indice pari a 4 e renderli accessibili a livello di programmazione usando `ExpensiveProductsPriceInBoldItalic.Rows(4)`. A questo punto voleste avere il contenuto dell'intera riga visualizzato in un tipo di carattere grassetto, corsivo usando il codice seguente:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample4.vb)]

Tuttavia, ciò renderà *entrambi* l'etichetta (prezzo) e il valore in grassetto e corsivo. Se si vuole rendere solo il valore in grassetto e corsivo che dobbiamo applicare questo codice di formattazione alla seconda cella nella riga, che può essere eseguita con i seguenti:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample5.vb)]

Poiché le esercitazioni finora utilizzato fogli di stile per mantenere una netta separazione tra il markup sottoposto a rendering e informazioni correlate allo stile, anziché impostare le proprietà di stile specifico, come illustrato in precedenza è possibile invece utilizzare una classe CSS. Aprire il `Styles.css` foglio di stile e aggiungere una nuova classe CSS denominata `ExpensivePriceEmphasis` con la definizione seguente:


[!code-css[Main](custom-formatting-based-upon-data-vb/samples/sample6.css)]

Quindi, nella `DataBound` gestore dell'evento, impostare la cella `CssClass` proprietà `ExpensivePriceEmphasis`. Il codice seguente illustra il `DataBound` gestore eventi nella loro interezza:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample7.vb)]

Quando si visualizzano Chai compare, che ha un costo inferiore a $75,00, viene visualizzato il prezzo di un tipo di carattere normale (vedere la figura 4). Tuttavia, quando si visualizzano Mishi Kobe Niku, che ha un prezzo di $97.00, il prezzo viene visualizzato in un tipo di carattere grassetto, corsivo (vedere la figura 5).


[![Prezzi di meno di $75,00 vengono visualizzati in un tipo di carattere normale](custom-formatting-based-upon-data-vb/_static/image9.png)](custom-formatting-based-upon-data-vb/_static/image8.png)

**Figura 4**: i prezzi minore di $75,00 vengono visualizzati in un tipo di carattere normale ([fare clic per visualizzare l'immagine con dimensioni normali](custom-formatting-based-upon-data-vb/_static/image10.png))


[![I prezzi Expensive Products vengono visualizzati in un grassetto, del carattere corsivo](custom-formatting-based-upon-data-vb/_static/image12.png)](custom-formatting-based-upon-data-vb/_static/image11.png)

**Figura 5**: i prezzi Expensive Products vengono visualizzati in un grassetto, corsivo carattere ([fare clic per visualizzare l'immagine con dimensioni normali](custom-formatting-based-upon-data-vb/_static/image13.png))


## <a name="using-the-formview-controlsdataboundevent-handler"></a>Uso del controllo FormView`DataBound`gestore dell'evento

I passaggi per determinare i dati sottostanti associati a un controllo FormView sono identici a quelle per creare un controllo DetailsView una `DataBound` gestore eventi, eseguire il cast di `DataItem` associata al controllo proprietà per il tipo di oggetto appropriato e determinare come procedere. Il controllo FormView e DetailsView sono diversi, tuttavia, in modalità di aggiornamento di aspetto della propria interfaccia utente.

Il controllo FormView non contiene alcun BoundField e pertanto non dispone di `Rows` raccolta. Al contrario, un controllo FormView è costituito da modelli, che possono contenere una combinazione di codice HTML statico, controlli Web e la sintassi di associazione dati. Modificando lo stile di un controllo FormView in genere implica la regolazione lo stile di uno o più dei controlli Web all'interno dei modelli del controllo FormView.

Per illustrare questo concetto, è possibile utilizzare un controllo FormView ai prodotti di elenco come illustrato nell'esempio precedente, ma questa volta è possibile visualizzare solo il nome del prodotto e le unità a magazzino con le unità a magazzino visualizzato in un carattere di colore rosso se è minore o uguale a 10.

## <a name="step-4-displaying-the-product-information-in-a-formview"></a>Passaggio 4: Visualizzare le informazioni sul prodotto in un controllo FormView

Aggiungere un controllo FormView per il `CustomColors.aspx` pagina sotto il controllo DetailsView e il set relativo `ID` proprietà `LowStockedProductsInRed`. Associare FormView al controllo ObjectDataSource creato nel passaggio precedente. Verrà creato un `ItemTemplate`, `EditItemTemplate`, e `InsertItemTemplate` per FormView. Rimuovere il `EditItemTemplate` e `InsertItemTemplate` e semplificare le `ItemTemplate` da includere solo il `ProductName` e `UnitsInStock` valori, ognuno nella proprio controlli etichetta denominata in modo appropriato. Analogamente a DetailsView dell'esempio precedente, controllare anche la casella di controllo Attiva Paging nello smart tag del controllo FormView.

Dopo tali modifiche al markup del controllo di FormView dovrebbe essere simile al seguente:


[!code-aspx[Main](custom-formatting-based-upon-data-vb/samples/sample8.aspx)]

Si noti che il `ItemTemplate` contiene:

- **Codice HTML statico** il testo "prodotto:" e "unità a magazzino:" con il `<br />` e `<b>` elementi.
- **Controlli Web** i due controlli Label `ProductNameLabel` e `UnitsInStockLabel`.
- **Sintassi di Data Binding** il `<%# Bind("ProductName") %>` e `<%# Bind("UnitsInStock") %>` sintassi, che assegna i valori di questi campi per i controlli di etichetta `Text` proprietà.

## <a name="step-5-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a>Passaggio 5: Determinare a livello di codice il valore dei dati nel gestore eventi associato a dati

Con markup del controllo FormView completa, il passaggio successivo consiste nel determinare a livello di programmazione se la `UnitsInStock` valore è minore o uguale a 10. Questa operazione viene eseguita in modo esatto con FormView come accadeva con DetailsView. Per iniziare, creare un gestore eventi per il controllo FormView `DataBound` evento.


![Creare il gestore eventi associato a dati](custom-formatting-based-upon-data-vb/_static/image14.png)

**Figura 6**: creare il `DataBound` gestore dell'evento


Nell'evento gestore eseguito il cast del controllo FormView `DataItem` proprietà per un `ProductsRow` dell'istanza e determinare se il `UnitsInPrice` dei valori è che occorre per visualizzarlo in un carattere di colore rosso.


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample9.vb)]

## <a name="step-6-formatting-the-unitsinstocklabel-label-control-in-the-formviews-itemtemplate"></a>Passaggio 6: Formattazione del controllo etichetta UnitsInStockLabel in ItemTemplate del controllo FormView

Il passaggio finale consiste nel formattare l'oggetto visualizzato `UnitsInStock` valore in un carattere di colore rosso se il valore è minore o uguale a 10. A tale scopo è necessario accedere a livello di `UnitsInStockLabel` controllare nel `ItemTemplate` e impostarne le proprietà di stile in modo che il testo viene visualizzato in rosso. Per accedere a un controllo Web in un modello, usare il `FindControl("controlID")` metodo simile al seguente:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample10.vb)]

In questo esempio si vuole accedere a un'etichetta di controllo la cui `ID` valore è `UnitsInStockLabel`, in modo che si utilizzerebbe:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample11.vb)]

Dopo aver ottenuto un riferimento a livello di codice per il controllo Web, è possibile modificarne le proprietà correlate allo stile in base alle esigenze. Come nell'esempio precedente, ho creato una classe CSS `Styles.css` denominato `LowUnitsInStockEmphasis`. Per applicare questo stile al controllo etichetta Web, impostare il `CssClass` proprietà conseguenza.


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample12.vb)]

> [!NOTE]
> La sintassi per la formattazione di un modello a livello di codice l'accesso al controllo Web utilizzando `FindControl("controlID")` e quindi impostare le proprietà correlate allo stile può anche essere usato quando si usa [TemplateFields](https://msdn.microsoft.com/library/system.web.ui.webcontrols.templatefield(VS.80).aspx) nel controllo DetailsView o GridView controlli. Verrà esaminato TemplateFields nell'esercitazione successiva.


Figure 7 mostra FormView durante la visualizzazione di un prodotto il cui `UnitsInStock` valore è maggiore di 10, mentre il prodotto nella figura 8 è il valore minore di 10.


[![Per i prodotti con un sufficientemente grande Units In Stock, viene applicata nessuna formattazione personalizzata](custom-formatting-based-upon-data-vb/_static/image16.png)](custom-formatting-based-upon-data-vb/_static/image15.png)

**Figura 7**: viene applicato per i prodotti con un sufficientemente grande Units In Stock, nessuna formattazione personalizzata ([fare clic per visualizzare l'immagine con dimensioni normali](custom-formatting-based-upon-data-vb/_static/image17.png))


[![Le unità in magazzino numero viene visualizzato in rosso per quelli prodotti con i valori di o minore di 10](custom-formatting-based-upon-data-vb/_static/image19.png)](custom-formatting-based-upon-data-vb/_static/image18.png)

**Figura 8**: l'unità in magazzino numero viene visualizzata in rosso per quelli prodotti con i valori 10 o minore di ([fare clic per visualizzare l'immagine con dimensioni normali](custom-formatting-based-upon-data-vb/_static/image20.png))


## <a name="formatting-with-the-gridviewsrowdataboundevent"></a>La formattazione con il controllo GridView`RowDataBound`evento

In precedenza sono stati esaminati la sequenza di passaggi DetailsView e FormView controlla lo stato di avanzamento tramite durante l'associazione dati. È possibile esaminare questi passaggi ancora una volta come un processo di aggiornamento.

1. I dati controllo Web `DataBinding` viene generato l'evento.
2. I dati vengono associati ai dati di controllo Web.
3. I dati controllo Web `DataBound` viene generato l'evento.

Questi tre semplici passaggi sono sufficienti per il controllo DetailsView e FormView perché contengono solo un singolo record. Per il controllo GridView, che consente di visualizzare *tutti* record associato a esso (non solo il primo), passaggio 2 è un po' più complesso.

Nel passaggio 2 GridView enumera l'origine dati e, per ogni record, viene creato un `GridViewRow` istanza ed esegue l'associazione del record corrente. Per ogni `GridViewRow` aggiunto a GridView, vengono generati due eventi:

- **`RowCreated`** viene attivato dopo il `GridViewRow` è stato creato
- **`RowDataBound`** viene generato dopo che il record corrente è stato associato ai `GridViewRow`.

Per GridView, quindi, l'associazione dati è descritta in modo più accurato dalla sequenza di passaggi seguente:

1. Il controllo GridView `DataBinding` viene generato l'evento.
2. L'associazione dati a GridView.   
  
   Per ogni record nell'origine dati 

    1. Creare un `GridViewRow` oggetto
    2. Attivare il `RowCreated` evento
    3. Associare il record per il `GridViewRow`
    4. Attivare il `RowDataBound` evento
    5. Aggiungere il `GridViewRow` per il `Rows` raccolta
3. Il controllo GridView `DataBound` viene generato l'evento.

Per personalizzare il formato dei singoli record di GridView, quindi, è necessario creare un gestore eventi per il `RowDataBound` evento. Per illustrare questo concetto, è possibile aggiungere un controllo GridView al `CustomColors.aspx` pagina che elenca il nome, categoria e prezzo per ogni prodotto, evidenziando i prodotti il cui prezzo è minore di $10,00 con un colore di sfondo giallo.

## <a name="step-7-displaying-product-information-in-a-gridview"></a>Passaggio 7: Visualizzazione di informazioni sul prodotto in un oggetto GridView

Aggiungere un controllo GridView sotto il controllo FormView nell'esempio precedente e impostare relativi `ID` proprietà `HighlightCheapProducts`. Poiché si dispone già di un ObjectDataSource che restituisce tutti i prodotti nella pagina, effettuare l'associazione di GridView. Infine, modificare i BoundField di GridView in modo da includere solo i nomi dei prodotti, categorie e i prezzi. Dopo tali modifiche al markup del controllo GridView dovrebbe essere simile:


[!code-aspx[Main](custom-formatting-based-upon-data-vb/samples/sample13.aspx)]

Figura 9 mostra lo stato di avanzamento fino a questo punto, quando viene visualizzato tramite un browser.


[![Il controllo GridView sono elencati il nome, categoria e prezzo per ogni prodotto](custom-formatting-based-upon-data-vb/_static/image22.png)](custom-formatting-based-upon-data-vb/_static/image21.png)

**Figura 9**: il controllo GridView sono elencati il nome, categoria e prezzo per ogni prodotto ([fare clic per visualizzare l'immagine con dimensioni normali](custom-formatting-based-upon-data-vb/_static/image23.png))


## <a name="step-8-programmatically-determining-the-value-of-the-data-in-the-rowdatabound-event-handler"></a>Passaggio 8: Determinare a livello di codice il valore dei dati nel gestore dell'evento RowDataBound

Quando la `ProductsDataTable` è associato a GridView relativi `ProductsRow` istanze vengono enumerati e per ogni `ProductsRow` un `GridViewRow` viene creato. Il `GridViewRow`del `DataItem` proprietà viene assegnato per la particolare `ProductRow`, dopo il quale il controllo GridView `RowDataBound` gestore dell'evento viene generato. Per determinare il `UnitPrice` associate a GridView. valore per ogni prodotto, quindi, è necessario creare un gestore eventi per il controllo GridView `RowDataBound` evento. In questo gestore eventi è possibile controllare la `UnitPrice` valore per l'oggetto corrente `GridViewRow` e prendere una decisione di formattazione per quella riga.

Questo gestore eventi può essere creato con la stessa serie di passaggi come FormView e DetailsView.


![Creare un gestore eventi per l'evento RowDataBound del controllo GridView](custom-formatting-based-upon-data-vb/_static/image24.png)

**Figura 10**: creare un gestore eventi per il controllo GridView `RowDataBound` evento


Creare il gestore dell'evento in questo modo provocherà il codice seguente per essere aggiunto automaticamente alla porzione di codice della pagina ASP.NET:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample14.vb)]

Quando la `RowDataBound` viene generato l'evento, il gestore dell'evento viene passato come secondo parametro un oggetto di tipo `GridViewRowEventArgs`, che include una proprietà denominata `Row`. Questa proprietà restituisce un riferimento di `GridViewRow` era solo i dati associati. Per l'accesso il `ProductsRow` istanza associata ai `GridViewRow` usiamo il `DataItem` proprietà come segue:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample15.vb)]

Quando si lavora con i `RowDataBound` gestore dell'evento è importante tenere presente che il controllo GridView è costituito da tipi diversi di righe e che questo evento viene generato per *tutti* i tipi di riga. Oggetto `GridViewRow`del tipo può essere determinato dal relativo `RowType` proprietà e può avere uno dei valori possibili:

- `DataRow` una riga a cui è associata a un record da di GridView `DataSource`
- `EmptyDataRow` la riga visualizzata se il controllo GridView `DataSource` è vuoto
- `Footer` la riga di piè di pagina. visualizzato se il controllo GridView `ShowFooter` è impostata su `True`
- `Header` la riga di intestazione. illustrato se GridView ShowHeader viene impostata su `True` (predefinito)
- `Pager` Per di GridView che implementano il paging, la riga che visualizza l'interfaccia di paging
- `Separator` non utilizzato per il controllo GridView, ma è usato dal `RowType` controlla le proprietà per DataList e Repeater, due dati controlli Web si parlerà in futuro le esercitazioni

Poiché il `EmptyDataRow`, `Header`, `Footer`, e `Pager` righe non sono associate a un `DataSource` record, sempre hanno un valore del `Nothing` per loro `DataItem` proprietà. Per questo motivo, prima di tentare di funzionare con l'attuale `GridViewRow`del `DataItem` proprietà, è necessario assicurarsi innanzitutto che stiamo affrontando una `DataRow`. Questa operazione può essere eseguita controllando la `GridViewRow`del `RowType` proprietà come segue:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample16.vb)]

## <a name="step-9-highlighting-the-row-yellow-when-the-unitprice-value-is-less-than-1000"></a>Passaggio 9: L'evidenziazione della riga gialla di UnitPrice valore When è minore di $10,00

L'ultimo passaggio consiste nel livello di codice evidenzia l'intera `GridViewRow` se il `UnitPrice` valore per tale riga è minore di $10,00. La sintassi per l'accesso alle righe o celle di un controllo GridView è analogo a quello di DetailsView `GridViewID.Rows(index)` per accedere alla riga intera, `GridViewID.Rows(index).Cells(index)` per accedere a una cella particolare. Tuttavia, quando la `RowDataBound` gestore dell'evento viene attivato l'associazione a dati `GridViewRow` deve ancora essere aggiunto al controllo GridView `Rows` raccolta. Di conseguenza non è possibile accedere corrente `GridViewRow` dell'istanza dal `RowDataBound` gestore eventi mediante la raccolta di righe.

Invece di `GridViewID.Rows(index)`, si può fare riferimento corrente `GridViewRow` dell'istanza nel `RowDataBound` gestore evento utilizzando `e.Row`. Vale a dire, per poter evidenziare corrente `GridViewRow` dell'istanza dal `RowDataBound` utilizzeremmo gestore dell'evento:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample17.vb)]

Anziché impostare il `GridViewRow`del `BackColor` proprietà direttamente, è possibile mantenere usando le classi CSS. Ho creato una classe CSS denominata `AffordablePriceEmphasis` che imposta il colore di sfondo su giallo. Completato `RowDataBound` gestore eventi seguente:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample18.vb)]


[![I prodotti più conveniente sono evidenziati giallo](custom-formatting-based-upon-data-vb/_static/image26.png)](custom-formatting-based-upon-data-vb/_static/image25.png)

**Figura 11**: la maggior parte di prodotti accessibili associate diventano gialle evidenziato ([fare clic per visualizzare l'immagine con dimensioni normali](custom-formatting-based-upon-data-vb/_static/image27.png))


## <a name="summary"></a>Riepilogo

In questa esercitazione è stato illustrato come formattare il controllo GridView, DetailsView e FormView in base ai dati associati al controllo. A tale scopo è stato creato un gestore eventi per il `DataBound` o `RowDataBound` eventi, in cui è stati esaminati i dati sottostanti insieme a una modifica della formattazione, se necessario. Per accedere ai dati associati a un controllo DetailsView o FormView, usiamo il `DataItem` proprietà nel `DataBound` gestore eventi; per un controllo GridView, ogni `GridViewRow` dell'istanza `DataItem` proprietà contiene i dati associati a tale riga, che è disponibile nel `RowDataBound` gestore dell'evento.

La sintassi per la modifica a livello di codice del controllo Web i dati di formattazione varia a seconda del controllo Web e come verranno visualizzati i dati da formattare. Per GridView e DetailsView controlli, le righe e celle sono accessibili da un indice ordinale. Per il controllo FormView, che utilizza i modelli, il `FindControl("controlID")` metodo viene comunemente utilizzato per individuare un controllo Web all'interno del modello.

Nell'esercitazione successiva esamineremo come utilizzare i modelli di GridView e DetailsView. Inoltre, si vedrà un'altra tecnica per personalizzare la formattazione in base ai dati sottostanti.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. I revisori per questa esercitazione sono state E.R. Morelli, Dennis Patterson e Dan Jagers. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](displaying-summary-information-in-the-gridview-s-footer-cs.md)
> [Successivo](using-templatefields-in-the-gridview-control-vb.md)
