---
uid: web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-vb
title: Formattazione personalizzata sulla base dei dati (VB) | Documenti Microsoft
author: rick-anderson
description: Modificare il formato di FormView in base ai dati a essa associati, GridView o DetailsView può essere eseguita in diversi modi. In questa esercitazione, ti invieremo l...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: df5a1525-386f-4632-972c-57b199870bc3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-vb
msc.type: authoredcontent
ms.openlocfilehash: a5c7f99b863697cc49a5bc9831dae861f51e129d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30876943"
---
<a name="custom-formatting-based-upon-data-vb"></a>Formattazione personalizzata sulla base dei dati (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare App di esempio](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_11_VB.exe) o [Scarica il PDF](custom-formatting-based-upon-data-vb/_static/datatutorial11vb1.pdf)

> Modificare il formato di FormView in base ai dati a essa associati, GridView o DetailsView può essere eseguita in diversi modi. In questa esercitazione verrà esaminato come eseguire associata ai dati tramite l'utilizzo di gestori di eventi associati a dati e RowDataBound di formattazione.


## <a name="introduction"></a>Introduzione

L'aspetto dei controlli GridView, DetailsView e FormView può essere personalizzato tramite numerose delle proprietà correlate allo stile. Le proprietà come `CssClass`, `Font`, `BorderWidth`, `BorderStyle`, `BorderColor`, `Width`, e `Height`, ad esempio, determinano l'aspetto generale del controllo viene eseguito il rendering. Le proprietà incluse `HeaderStyle`, `RowStyle`, `AlternatingRowStyle`, e altre consentono le stesse impostazioni di stile da applicare a particolari sezioni. Analogamente, queste impostazioni di stile possono essere applicate a livello di campo.

In molti scenari, tuttavia, i requisiti di formattazione dipendono dal valore dei dati visualizzati. Per richiamare l'attenzione per fuori stock prodotti, ad esempio, un elenco di informazioni sul prodotto potrebbe impostare il colore di sfondo su giallo per i prodotti il cui `UnitsInStock` e `UnitsOnOrder` campi sono entrambi uguali a 0. Per evidenziare i prodotti più costosi, si può decidere di visualizzare i prezzi dei prodotti costi più 75,00 in grassetto.

Modificare il formato di FormView in base ai dati a essa associati, GridView o DetailsView può essere eseguita in diversi modi. In questa esercitazione viene illustrato come eseguire una formattazione dei dati associati mediante l'utilizzo del `DataBound` e `RowDataBound` gestori eventi. Nella prossima esercitazione verrà illustrato un approccio alternativo.

## <a name="using-the-detailsview-controlsdataboundevent-handler"></a>Utilizzo del controllo DetailsView`DataBound`gestore dell'evento

Se dati sono associati a un controllo DetailsView, da un controllo origine dati o mediante l'assegnazione a livello di programmazione di dati per il controllo `DataSource` proprietà e la chiamata relativo `DataBind()` si verificano di metodo, la sequenza di passaggi seguente:

1. I dati controllo Web `DataBinding` viene generato l'evento.
2. I dati vengono associati ai dati di controllo Web.
3. I dati controllo Web `DataBound` viene generato l'evento.

La logica personalizzata può essere inserita subito dopo i passaggi 1 e 3 tramite un gestore eventi. Tramite la creazione di un gestore eventi per il `DataBound` eventi è possibile determinare a livello di programmazione i dati che sono stata associata al controllo dati Web e modificare la formattazione in base alle esigenze. A questo scopo si crea un controllo DetailsView che elenca le informazioni generali su un prodotto, ma verrà visualizzato il `UnitPrice` valore in un ***carattere grassetto, corsivo*** è superiore a $75,00.

## <a name="step-1-displaying-the-product-information-in-a-detailsview"></a>Passaggio 1: Visualizzare le informazioni sul prodotto in un controllo DetailsView.

Aprire il `CustomColors.aspx` nella pagina il `CustomFormatting` cartella, trascinare un controllo DetailsView dalla casella degli strumenti nella finestra di progettazione, impostare il relativo `ID` valore della proprietà da `ExpensiveProductsPriceInBoldItalic`e associarlo a un nuovo controllo ObjectDataSource che richiama la `ProductsBLL` della classe `GetProducts()` metodo. Per eseguire questa procedura dettagliata è stati omessi qui per ragioni di brevità poiché essi abbiamo esaminato in dettaglio nelle esercitazioni precedenti.

Una volta ObjectDataSource è stato associato al controllo DetailsView., è opportuno modificare l'elenco dei campi. È stato scelto di rimuovere il `ProductID`, `SupplierID`, `CategoryID`, `UnitsInStock`, `UnitsOnOrder`, `ReorderLevel`, e `Discontinued` BoundField e rinominato e riformattato restanti BoundField. Inoltre eliminate le `Width` e `Height` impostazioni. Poiché il controllo DetailsView viene visualizzato un solo record, è necessario abilitare il paging per consentire all'utente finale di visualizzare tutti i prodotti. Farlo selezionando la casella di controllo Abilita Paging nello smart tag del controllo DetailsView.


[![Figura 1: La casella Abilita Paging nello Smart Tag del controllo DetailsView.](custom-formatting-based-upon-data-vb/_static/image2.png)](custom-formatting-based-upon-data-vb/_static/image1.png)

**Figura 1**: nella figura 1: abilitare il Paging la casella di controllo di DetailsView Smart tag ([fare clic per visualizzare l'immagine ingrandita](custom-formatting-based-upon-data-vb/_static/image3.png))


Dopo aver apportato queste modifiche, il markup di DetailsView sarà:


[!code-aspx[Main](custom-formatting-based-upon-data-vb/samples/sample1.aspx)]

È opportuno testare questa pagina nel browser.


[![Il controllo DetailsView visualizza un solo prodotto alla volta](custom-formatting-based-upon-data-vb/_static/image5.png)](custom-formatting-based-upon-data-vb/_static/image4.png)

**Figura 2**: il controllo DetailsView controllo consente di visualizzare un prodotto in un momento ([fare clic per visualizzare l'immagine ingrandita](custom-formatting-based-upon-data-vb/_static/image6.png))


## <a name="step-2-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a>Passaggio 2: Determinare a livello di codice il valore dei dati nel gestore eventi associato a dati

Per visualizzare il prezzo di un tipo di carattere grassetto, corsivo per i prodotti il cui `UnitPrice` valore maggiore di $75,00, è necessario essere in grado di determinare a livello di codice prima di `UnitPrice` valore. Per il controllo DetailsView, può essere eseguita nel `DataBound` gestore dell'evento. Per creare l'evento gestore fare clic sul controllo DetailsView. nella finestra di progettazione, quindi passare alla finestra Proprietà. Premere F4 per attivare la modalità, se non è visibile o passare al menu Visualizza e selezionare l'opzione del menu Finestra proprietà. Dalla finestra delle proprietà, scegliere l'icona del fulmine per elencare gli eventi di DetailsView. Successivamente, fare doppio clic su di `DataBound` evento o digitare il nome che si desidera creare il gestore dell'evento.


![Creare un gestore eventi per l'evento associato a dati](custom-formatting-based-upon-data-vb/_static/image7.png)

**Figura 3**: creare un gestore eventi per il `DataBound` evento


> [!NOTE]
> È anche possibile creare un gestore eventi da parte di codice della pagina ASP.NET. Sono disponibili due elenchi a discesa nella parte superiore della pagina. Selezionare l'oggetto dall'elenco a discesa a sinistra e l'evento che si desidera creare un gestore per dall'elenco a discesa a destra e Visual Studio crea automaticamente il gestore eventi appropriato.


In questo modo verrà automaticamente creato il gestore dell'evento e di passare alla parte di codice in cui è stato aggiunto. A questo punto verrà visualizzato:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample2.vb)]

I dati associati al controllo DetailsView è possibile accedere tramite il `DataItem` proprietà. Tenere presente che viene eseguita l'associazione i controlli a un oggetto DataTable fortemente tipizzato è costituito da una raccolta di istanze di DataRow fortemente tipizzata. Quando l'oggetto DataTable è associato al controllo DetailsView., il primo DataRow in DataTable viene assegnato al controllo DetailsView `DataItem` proprietà. In particolare, il `DataItem` proprietà viene assegnato un `DataRowView` oggetto. È possibile utilizzare il `DataRowView`del `Row` proprietà per ottenere l'accesso all'oggetto DataRow sottostante, che è effettivamente un `ProductsRow` istanza. Una volta che questo `ProductsRow` istanza contribuire a migliorare la nostra decisione controllando semplicemente i valori delle proprietà dell'oggetto.

Il codice seguente viene illustrato come determinare se il `UnitPrice` valore associata al controllo DetailsView è maggiore di $75,00:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample3.vb)]

> [!NOTE]
> Poiché `UnitPrice` può avere un `NULL` valore nel database, controllare innanzitutto assicurarsi che ci stiamo non si tratta di un `NULL` valore prima di accedere il `ProductsRow`del `UnitPrice` proprietà. Questo controllo è importante perché se si tenta di accedere il `UnitPrice` proprietà quando dispone di un `NULL` valore il `ProductsRow` oggetto genererà un [StrongTypingException eccezione](https://msdn.microsoft.com/library/system.data.strongtypingexception.aspx).


## <a name="step-3-formatting-the-unitprice-value-in-the-detailsview"></a>Passaggio 3: Formattazione del valore di UnitPrice nel controllo DetailsView.

A questo punto è possibile determinare se il `UnitPrice` associato al controllo DetailsView ha un valore che supera i $75,00, ma è stata ancora per vedere come modificare a livello di programmazione di DetailsView le formattazione di conseguenza. Per modificare la formattazione di una riga nel controllo DetailsView, accedere a livello di codice alla riga utilizzando `DetailsViewID.Rows(index)`; per modificare una cella particolare, accedere utilizzare `DetailsViewID.Rows(index).Cells(index)`. Dopo aver ottenuto un riferimento alla riga o cella è quindi possibile modificare l'aspetto impostandone le proprietà correlate allo stile.

L'accesso a una riga a livello di codice, è necessario conoscere l'indice della riga, che inizia da 0. Il `UnitPrice` riga è la quinta riga nel controllo DetailsView, assegnazione di un indice pari a 4 e renderli accessibili a livello di programmazione utilizzando `ExpensiveProductsPriceInBoldItalic.Rows(4)`. A questo punto è stato possibile dispone il contenuto dell'intera riga visualizzato in un tipo di carattere grassetto, corsivo usando il codice seguente:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample4.vb)]

Tuttavia, in questo modo *entrambi* l'etichetta (Price) e il valore in grassetto e corsivo. Se lo si desidera rendere solo il valore in grassetto e corsivo che è necessario applicare la formattazione alla seconda cella nella riga, che può essere eseguita con i seguenti:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample5.vb)]

Poiché le esercitazioni finora utilizzato fogli di stile per mantenere una netta separazione tra i markup sottoposto a rendering e le informazioni relative allo stile, anziché l'impostazione delle proprietà di stile specifico, come illustrato in precedenza verrà invece utilizzare una classe CSS. Aprire il `Styles.css` foglio di stile e aggiungere una nuova classe CSS denominata `ExpensivePriceEmphasis` con la definizione seguente:


[!code-css[Main](custom-formatting-based-upon-data-vb/samples/sample6.css)]

Quindi, nel `DataBound` gestore dell'evento, impostare la cella `CssClass` proprietà `ExpensivePriceEmphasis`. Il codice seguente illustra il `DataBound` gestore dell'evento nella sua interezza:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample7.vb)]

Quando si visualizzano Chai, che ha un costo inferiore a $75,00, il prezzo viene visualizzato in un tipo di carattere normale (vedere la figura 4). Tuttavia, quando si visualizzano Mishi Kobe Niku, con un prezzo di $97.00, il prezzo viene visualizzato in un tipo di carattere grassetto, corsivo (vedere Figura 5).


[![I prezzi minore di $75,00 vengono visualizzati in un tipo di carattere normale](custom-formatting-based-upon-data-vb/_static/image9.png)](custom-formatting-based-upon-data-vb/_static/image8.png)

**Figura 4**: i prezzi minore di $75,00 vengono visualizzati in un tipo di carattere normale ([fare clic per visualizzare l'immagine ingrandita](custom-formatting-based-upon-data-vb/_static/image10.png))


[![I prezzi dei prodotti costosi vengono visualizzati in un grassetto, corsivo del tipo di carattere](custom-formatting-based-upon-data-vb/_static/image12.png)](custom-formatting-based-upon-data-vb/_static/image11.png)

**Figura 5**: i prezzi dei prodotti costosi vengono visualizzati in un grassetto, corsivo carattere ([fare clic per visualizzare l'immagine ingrandita](custom-formatting-based-upon-data-vb/_static/image13.png))


## <a name="using-the-formview-controlsdataboundevent-handler"></a>Utilizzo del controllo FormView`DataBound`gestore dell'evento

I passaggi per determinare i dati sottostanti associati a un controllo FormView sono identici a quelli per la creazione di un controllo DetailsView un `DataBound` gestore eventi, eseguire il cast di `DataItem` proprietà al tipo di oggetto appropriato associato al controllo e stabilire come procedere. Le FormView e DetailsView diversi, tuttavia, in modalità di aggiornamento aspetto dell'interfaccia utente.

Il controllo FormView non contiene alcun BoundField e pertanto non dispone di `Rows` insieme. Al contrario, un controllo FormView è costituita da modelli, che possono contenere una combinazione di HTML statico, i controlli Web e la sintassi di associazione dati. Modificare lo stile di un controllo FormView in genere, è necessario modificare lo stile di uno o più dei controlli Web all'interno dei modelli del controllo FormView.

Per illustrare questo concetto, si usa un FormView ai prodotti di elenco come nell'esempio precedente, ma questa volta si visualizzare solo il nome del prodotto e l'unità in magazzino con le unità in magazzino visualizzati in colore rosso se il valore è minore o uguale a 10.

## <a name="step-4-displaying-the-product-information-in-a-formview"></a>Passaggio 4: Visualizzare le informazioni sul prodotto in un controllo FormView

Aggiungere un controllo FormView per il `CustomColors.aspx` pagina sotto il controllo DetailsView e il set relativo `ID` proprietà `LowStockedProductsInRed`. Associare il controllo FormView al controllo ObjectDataSource creato nel passaggio precedente. Verrà creato un `ItemTemplate`, `EditItemTemplate`, e `InsertItemTemplate` per FormView. Rimuovere il `EditItemTemplate` e `InsertItemTemplate` e semplificare la `ItemTemplate` da includere solo la `ProductName` e `UnitsInStock` valori, ognuno dei propri controlli etichetta denominato in modo appropriato. Come con DetailsView dell'esempio precedente, anche la casella di controllo Abilita Paging nello smart tag del controllo FormView.

Dopo queste modifiche al markup del FormView dovrebbe essere simile al seguente:


[!code-aspx[Main](custom-formatting-based-upon-data-vb/samples/sample8.aspx)]

Si noti che la `ItemTemplate` contiene:

- **HTML statico** il testo "prodotto:" e "unità In magazzino:" con il `<br />` e `<b>` elementi.
- **Controlli Web** i due controlli etichetta `ProductNameLabel` e `UnitsInStockLabel`.
- **Sintassi DataBinding** il `<%# Bind("ProductName") %>` e `<%# Bind("UnitsInStock") %>` sintassi, che assegna i valori di questi campi per i controlli di etichetta `Text` proprietà.

## <a name="step-5-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a>Passaggio 5: Determinare a livello di codice il valore dei dati nel gestore eventi associato a dati

Con il markup del controllo FormView è completo, il passaggio successivo consiste nel determinare a livello di codice se il `UnitsInStock` valore è minore o uguale a 10. Questa operazione viene eseguita in modo esatto con FormView e controllo DetailsView. Iniziare creando un gestore eventi per il controllo FormView `DataBound` evento.


![Creare il gestore dell'evento associato a dati](custom-formatting-based-upon-data-vb/_static/image14.png)

**Figura 6**: creare il `DataBound` gestore dell'evento


Nell'evento gestore eseguire il cast del controllo FormView `DataItem` proprietà per un `ProductsRow` istanza e determinare se il `UnitsInPrice` dei valori è che è necessario per visualizzarlo in un carattere di colore rosso.


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample9.vb)]

## <a name="step-6-formatting-the-unitsinstocklabel-label-control-in-the-formviews-itemtemplate"></a>Passaggio 6: Formattazione del controllo etichetta UnitsInStockLabel in ItemTemplate del controllo FormView

Il passaggio finale consiste nel formato visualizzato `UnitsInStock` valore in un carattere di colore rosso se il valore inferiore o uguale a 10. A tale scopo è necessario accedere a livello di `UnitsInStockLabel` controllo il `ItemTemplate` e impostarne le proprietà di stile in modo che il testo viene visualizzato in rosso. Per accedere a un controllo Web in un modello, utilizzare il `FindControl("controlID")` metodo simile al seguente:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample10.vb)]

Per questo esempio si desidera accedere a un'etichetta a controllo cui `ID` valore `UnitsInStockLabel`, pertanto è possibile utilizzare:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample11.vb)]

Dopo aver ottenuto un riferimento a livello di codice per il controllo Web, è possibile modificare le proprietà correlate allo stile in base alle esigenze. Come nell'esempio precedente, ho creato una classe CSS in `Styles.css` denominato `LowUnitsInStockEmphasis`. Per applicare questo stile al controllo etichetta Web, impostare il relativo `CssClass` proprietà conseguenza.


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample12.vb)]

> [!NOTE]
> La sintassi per la formattazione di un modello di accesso a livello di codice il controllo Web usando `FindControl("controlID")` e quindi impostando le proprietà correlate allo stile può essere utilizzato anche quando si utilizza [TemplateFields](https://msdn.microsoft.com/library/system.web.ui.webcontrols.templatefield(VS.80).aspx) in GridView o controllo DetailsView. controlli. Si esamineranno TemplateFields nell'esercitazione successiva.


Le figure 7 mostra FormView durante la visualizzazione di un prodotto il cui `UnitsInStock` valore è maggiore di 10, mentre il prodotto nella figura 8 è il valore minore di 10.


[![Per i prodotti con un sufficientemente grande Units In Stock, viene applicata nessuna formattazione personalizzata](custom-formatting-based-upon-data-vb/_static/image16.png)](custom-formatting-based-upon-data-vb/_static/image15.png)

**Figura 7**: viene applicata per i prodotti con un sufficientemente grande Units In Stock, nessuna formattazione personalizzata ([fare clic per visualizzare l'immagine ingrandita](custom-formatting-based-upon-data-vb/_static/image17.png))


[![L'unità in magazzino numero viene visualizzato in rosso per i prodotti con valori di 10 o meno](custom-formatting-based-upon-data-vb/_static/image19.png)](custom-formatting-based-upon-data-vb/_static/image18.png)

**Figura 8**: l'unità in magazzino numero viene visualizzata in rosso per i prodotti con valori di 10 o meno ([fare clic per visualizzare l'immagine ingrandita](custom-formatting-based-upon-data-vb/_static/image20.png))


## <a name="formatting-with-the-gridviewsrowdataboundevent"></a>La formattazione con il controllo GridView`RowDataBound`evento

In precedenza sono state esaminate la sequenza di passaggi di DetailsView e FormView Controlla stato di avanzamento tramite durante l'associazione dati. Verrà ora esaminato su questi passaggi nuovamente come un aggiornamento.

1. I dati controllo Web `DataBinding` viene generato l'evento.
2. I dati vengono associati ai dati di controllo Web.
3. I dati controllo Web `DataBound` viene generato l'evento.

Questi tre semplici passaggi sono sufficienti per il controllo DetailsView e FormView perché contengono solo un singolo record. Per GridView, che consente di visualizzare *tutti* record associato a tale (non solo il primo), il passaggio 2 è un po' più complicata.

Nel passaggio 2 GridView enumera l'origine dati e, per ogni record, crea un `GridViewRow` istanza e associa il record corrente. Per ogni `GridViewRow` aggiunto a GridView, vengono generati due eventi:

- **`RowCreated`** viene generato dopo il `GridViewRow` è stato creato
- **`RowDataBound`** viene generato dopo il record corrente è stato associato per il `GridViewRow`.

Per GridView, quindi, associazione dati è descritto in modo più accurato per la sequenza di passaggi seguente:

1. Il GridView `DataBinding` viene generato l'evento.
2. I dati sono associati a GridView.   
  
   Per ogni record nell'origine dati 

    1. Creare un `GridViewRow` oggetto
    2. Attivare il `RowCreated` evento
    3. Associare il record per il `GridViewRow`
    4. Attivare il `RowDataBound` evento
    5. Aggiungere il `GridViewRow` per il `Rows` raccolta
3. Il GridView `DataBound` viene generato l'evento.

Per personalizzare il formato di singoli record di GridView, quindi, è necessario creare un gestore eventi per il `RowDataBound` evento. A questo scopo, aggiungere un controllo GridView per il `CustomColors.aspx` pagina che elenca il nome, categoria e prezzo per ogni prodotto, evidenziando i prodotti il cui prezzo è inferiore a $10.00 con un colore di sfondo giallo.

## <a name="step-7-displaying-product-information-in-a-gridview"></a>Passaggio 7: Visualizzazione di informazioni sui prodotti in un controllo GridView.

Aggiungere un controllo GridView sotto il controllo FormView dell'esempio precedente e impostare il relativo `ID` proprietà `HighlightCheapProducts`. Poiché è già disponibile un ObjectDataSource che restituisce tutti i prodotti nella pagina, effettuare l'associazione GridView. Infine, modificare BoundField di GridView in modo da includere solo i nomi dei prodotti, categorie e i prezzi. Dopo queste modifiche al markup del controllo GridView dovrebbe essere simile:


[!code-aspx[Main](custom-formatting-based-upon-data-vb/samples/sample13.aspx)]

Stato di avanzamento a questo punto, quando viene visualizzato tramite un browser è illustrato nella figura 9.


[![GridView Elenca il nome, categoria e prezzo per ogni prodotto](custom-formatting-based-upon-data-vb/_static/image22.png)](custom-formatting-based-upon-data-vb/_static/image21.png)

**Figura 9**: GridView Elenca il nome, categoria e prezzo per ogni prodotto ([fare clic per visualizzare l'immagine ingrandita](custom-formatting-based-upon-data-vb/_static/image23.png))


## <a name="step-8-programmatically-determining-the-value-of-the-data-in-the-rowdatabound-event-handler"></a>Passaggio 8: Determinare a livello di codice il valore dei dati nel gestore eventi RowDataBound

Quando il `ProductsDataTable` è associato a GridView relativo `ProductsRow` istanze vengono enumerati e per ogni `ProductsRow` un `GridViewRow` viene creato. Il `GridViewRow`del `DataItem` proprietà viene assegnato per la particolare `ProductRow`, dopo il quale il controllo GridView `RowDataBound` gestore dell'evento viene generato. Per determinare il `UnitPrice` valore per ogni prodotto associato a GridView, quindi, è necessario creare un gestore eventi per il controllo GridView `RowDataBound` evento. In questo gestore eventi è possibile controllare il `UnitPrice` valore per l'oggetto corrente `GridViewRow` e prendere una decisione di formattazione per la riga.

Questo gestore eventi può essere creato utilizzando le stesse serie di passaggi con FormView e DetailsView.


![Creare un gestore eventi per l'evento RowDataBound del controllo GridView](custom-formatting-based-upon-data-vb/_static/image24.png)

**Figura 10**: creare un gestore eventi per il controllo GridView `RowDataBound` evento


Creare il gestore dell'evento in questo modo genererà il codice automaticamente da aggiungere alla parte di codice della pagina ASP.NET seguente:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample14.vb)]

Quando il `RowDataBound` viene generato l'evento, il gestore dell'evento viene passato come secondo parametro come oggetto di tipo `GridViewRowEventArgs`, che include una proprietà denominata `Row`. Questa proprietà restituisce un riferimento di `GridViewRow` che è stato solo i dati associati. Per l'accesso di `ProductsRow` istanza associata al `GridViewRow` utilizziamo la `DataItem` proprietà come illustrato di seguito:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample15.vb)]

Quando si lavora con la `RowDataBound` gestore eventi è importante tenere presente che il controllo GridView è costituito da diversi tipi di righe e che questo evento viene generato per *tutti* tipi di riga. A `GridViewRow`del tipo può essere determinato dal relativo `RowType` , proprietà e può avere uno dei valori possibili:

- `DataRow` una riga a cui è associata a un record di GridView `DataSource`
- `EmptyDataRow` la riga visualizzata se il controllo GridView `DataSource` è vuoto
- `Footer` la riga di piè di pagina. visualizzato se il controllo GridView `ShowFooter` è impostata su `True`
- `Header` la riga di intestazione; illustrato se GridView ShowHeader è impostata su `True` (impostazione predefinita)
- `Pager` per GridView che implementano il paging, la riga che visualizza l'interfaccia di paging
- `Separator` non utilizzata per GridView, ma per il `RowType` consentono di controllare le proprietà per il DataList e Repeater, due dati controlli Web verranno discussi in futuro esercitazioni

Poiché il `EmptyDataRow`, `Header`, `Footer`, e `Pager` righe non sono associate a un `DataSource` record, verrà sempre hanno un valore di `Nothing` per loro `DataItem` proprietà. Per questo motivo, prima di tentare di utilizzare corrente `GridViewRow`del `DataItem` proprietà, è innanzitutto necessario assicurarsi che ci stiamo occupa un `DataRow`. Questa operazione può essere eseguita controllando la `GridViewRow`del `RowType` proprietà come illustrato di seguito:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample16.vb)]

## <a name="step-9-highlighting-the-row-yellow-when-the-unitprice-value-is-less-than-1000"></a>Passaggio 9: Evidenziare la riga gialla quando il valore UnitPrice è inferiore a $10.00

L'ultimo passaggio consiste nel livello di codice evidenziare l'intero `GridViewRow` se il `UnitPrice` valore per tale riga è inferiore a $10.00. La sintassi per l'accesso alle righe o celle di un controllo GridView è lo stesso di DetailsView `GridViewID.Rows(index)` per l'intera riga, di accedere a `GridViewID.Rows(index).Cells(index)` per accedere a una cella particolare. Tuttavia, quando il `RowDataBound` gestore dell'evento viene generato l'associazione a dati `GridViewRow` deve ancora essere aggiunto al controllo GridView `Rows` insieme. Pertanto non è possibile accedere corrente `GridViewRow` istanza il `RowDataBound` gestore dell'evento tramite la raccolta di righe.

Invece di `GridViewID.Rows(index)`, si può fare riferimento corrente `GridViewRow` dell'istanza nel `RowDataBound` gestore eventi utilizzando `e.Row`. Ovvero, in ordine per evidenziare corrente `GridViewRow` istanza il `RowDataBound` verrà utilizzato un gestore dell'evento:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample17.vb)]

Anziché impostare la `GridViewRow`del `BackColor` proprietà direttamente, ci soffermeremo con le classi CSS. Ho creato una classe CSS denominata `AffordablePriceEmphasis` che imposta il colore di sfondo giallo. Completato `RowDataBound` gestore eventi segue:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample18.vb)]


[![I prodotti più conveniente sono giallo evidenziato](custom-formatting-based-upon-data-vb/_static/image26.png)](custom-formatting-based-upon-data-vb/_static/image25.png)

**Figura 11**: la maggior parte del conveniente prodotti sono giallo evidenziato ([fare clic per visualizzare l'immagine ingrandita](custom-formatting-based-upon-data-vb/_static/image27.png))


## <a name="summary"></a>Riepilogo

In questa esercitazione è stato illustrato come formattare il GridView, DetailsView e in base ai dati associati al controllo FormView. A tale scopo viene creato un gestore eventi per il `DataBound` o `RowDataBound` eventi, in cui è stati esaminati i dati sottostanti insieme a una modifica della formattazione, se necessario. Per accedere ai dati associati a un controllo DetailsView o FormView, utilizziamo la `DataItem` proprietà il `DataBound` gestore eventi; per un controllo GridView, ogni `GridViewRow` dell'istanza `DataItem` proprietà contiene i dati associati a tale riga, che è disponibile nel `RowDataBound` gestore dell'evento.

La sintassi per la configurazione a livello di codice la formattazione del controllo Web dati dipende dal controllo Web e la modalità di visualizzazione dei dati da formattare. Per DetailsView e GridView controlli, le righe e celle sono accessibili da un indice ordinale. Per il controllo FormView, che utilizza i modelli, il `FindControl("controlID")` metodo viene comunemente utilizzato per individuare un controllo Web da all'interno del modello.

Nella prossima esercitazione verrà esaminato come utilizzare i modelli con GridView e DetailsView. Inoltre, si vedrà che un'altra tecnica per personalizzare la formattazione in base ai dati sottostanti.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Lead revisori per questa esercitazione sono stati E.R. Morelli, Dennis Patterson e Dan Jagers. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](displaying-summary-information-in-the-gridview-s-footer-cs.md)
> [Successivo](using-templatefields-in-the-gridview-control-vb.md)
