---
uid: web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-detailsview-control-vb
title: Uso di TemplateFields nel controllo DetailsView (VB) | Microsoft Docs
author: rick-anderson
description: Le stesse funzionalità di TemplateFields disponibili con il controllo GridView sono anche disponibili con il controllo DetailsView. In questa esercitazione verrà visualizzato un unico prodotto...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 0b91d5f8-127d-4f6a-b204-f2e2b35ef703
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-detailsview-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 29a73a5f22048cf3a80d9f7d23c0a4a7b89882f0
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365393"
---
<a name="using-templatefields-in-the-detailsview-control-vb"></a>Uso di TemplateFields nel controllo DetailsView (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'App di esempio](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_13_VB.exe) o [Scarica il PDF](using-templatefields-in-the-detailsview-control-vb/_static/datatutorial13vb1.pdf)

> Le stesse funzionalità di TemplateFields disponibili con il controllo GridView sono anche disponibili con il controllo DetailsView. In questa esercitazione verrà visualizzato un solo prodotto alla volta usando un controllo DetailsView contenente TemplateFields.


## <a name="introduction"></a>Introduzione

Il TemplateField offre un livello superiore di flessibilità nei dati di rendering i BoundField, CampoCasellaDiControllo, HyperLinkField e di altri controlli di campo dati. Nel [esercitazione precedente](using-templatefields-in-the-gridview-control-vb.md) è stata esaminata tramite il TemplateField in GridView per:

- Visualizzare più valori di campo di dati in una colonna. In particolare, entrambi i `FirstName` e `LastName` campi sono stati combinati in una colonna GridView.
- Usare un controllo Web alternativo per esprimere un valore del campo dati. È stato illustrato come visualizzare il `HiredDate` valore usando un controllo calendario.
- Mostra le informazioni sullo stato in base ai dati sottostanti. Mentre il `Employees` tabella non contiene una colonna che restituisce il numero di giorni di un dipendente ha completato il processo, siamo in grado di visualizzare tali informazioni nell'esempio GridView nell'esercitazione precedente con l'uso di un TemplateField e un metodo di formattazione.

Le stesse funzionalità di TemplateFields disponibili con il controllo GridView sono anche disponibili con il controllo DetailsView. In questa esercitazione verrà visualizzato un solo prodotto alla volta usando un controllo DetailsView contenente due TemplateFields. Il primo TemplateField combinerà i `UnitPrice`, `UnitsInStock`, e `UnitsOnOrder` campi di dati in un'unica riga DetailsView. Il secondo TemplateField verrà visualizzato il valore del `Discontinued` campo, ma userà un metodo di formattazione per visualizzare "Sì" se `Discontinued` è `True`e "NO" in caso contrario.


[![Due TemplateFields vengono usati per personalizzare la visualizzazione](using-templatefields-in-the-detailsview-control-vb/_static/image2.png)](using-templatefields-in-the-detailsview-control-vb/_static/image1.png)

**Figura 1**: due TemplateFields vengono usati per personalizzare la visualizzazione ([fare clic per visualizzare l'immagine con dimensioni normali](using-templatefields-in-the-detailsview-control-vb/_static/image3.png))


Iniziamo!

## <a name="step-1-binding-the-data-to-the-detailsview"></a>Passaggio 1: Associazione dei dati in DetailsView

Come illustrato nell'esercitazione precedente, quando si lavora con TemplateFields è spesso più semplice iniziare creando il controllo DetailsView contenente solo i BoundField e quindi aggiungere di nuovo TemplateFields o convertire i BoundField esistente in TemplateFields come necessario . Pertanto, iniziare questa esercitazione aggiungendo un controllo DetailsView alla pagina tramite la finestra di progettazione e associarlo a ObjectDataSource che restituisce l'elenco dei prodotti. Questa procedura verrà creato un controllo DetailsView con BoundField per ognuno dei campi dei valori non booleani del prodotto e un CampoCasellaDiControllo per quel campo valore booleano (sospeso).

Aprire il `DetailsViewTemplateField.aspx` pagina e trascinare un controllo DetailsView dalla casella degli strumenti nella finestra di progettazione. Dallo smart tag di DetailsView scegliere di aggiungere un nuovo controllo ObjectDataSource che richiama il `ProductsBLL` della classe `GetProducts()` (metodo).


[![Aggiungere un nuovo controllo ObjectDataSource che richiama il metodo GetProducts()](using-templatefields-in-the-detailsview-control-vb/_static/image5.png)](using-templatefields-in-the-detailsview-control-vb/_static/image4.png)

**Figura 2**: aggiungere un nuovo controllo ObjectDataSource tale Invoke il `GetProducts()` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](using-templatefields-in-the-detailsview-control-vb/_static/image6.png))


Per questo report, rimuovere il `ProductID`, `SupplierID`, `CategoryID`, e `ReorderLevel` BoundField. Successivamente, riordinare i BoundField in modo che il `CategoryName` e `SupplierName` BoundField vengono visualizzati immediatamente dopo il `ProductName` BoundField. È possibile regolare la `HeaderText` proprietà e le proprietà di formattazione per i BoundField come si preferisce. Come in GridView, queste modifiche a livello BoundField possono essere eseguite tramite la finestra di dialogo campi (accessibile facendo clic sul collegamento Modifica campi nello smart tag di DetailsView) o tramite la sintassi dichiarativa. Infine, cancelliamo DetailsView `Height` e `Width` i valori delle proprietà per consentire il controllo DetailsView di controllo per espandere in base ai dati visualizzati e selezionare la casella di controllo Attiva Paging nello smart tag.

Dopo aver apportato queste modifiche, markup dichiarativo del controllo DetailsView dovrebbe essere simile al seguente:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample1.aspx)]

Si consiglia di visualizzare la pagina tramite un browser. A questo punto dovrebbe essere un singolo prodotto elencato (Chai) con le righe che mostra il nome del prodotto, categoria, fornitore, prezzo, unità a magazzino, quantità ordinata e il relativo stato non più utilizzato.


[![I dettagli del prodotto vengono visualizzati tramite una serie di BoundField](using-templatefields-in-the-detailsview-control-vb/_static/image8.png)](using-templatefields-in-the-detailsview-control-vb/_static/image7.png)

**Figura 3**: vengono visualizzati i dettagli del prodotto di utilizzando una serie di BoundField ([fare clic per visualizzare l'immagine con dimensioni normali](using-templatefields-in-the-detailsview-control-vb/_static/image9.png))


## <a name="step-2-combining-the-price-units-in-stock-and-units-on-order-into-one-row"></a>Passaggio 2: Combinazione di prezzo, unità a magazzino e quantità ordinata in una riga

DetailsView dispone di una riga per il `UnitPrice`, `UnitsInStock`, e `UnitsOnOrder` campi. Sarà possibile combinare questi campi di dati in una singola riga con un TemplateField aggiungendo un nuovo TemplateField o convertendo uno dell'oggetto esistente `UnitPrice`, `UnitsInStock`, e `UnitsOnOrder` BoundField in un TemplateField. Sebbene personalmente preferisco conversione BoundField esistente, è possibile esercitarsi aggiungendo un TemplateField nuovo.

Avviare facendo clic sul collegamento Modifica campi nello smart tag di DetailsView per visualizzare la finestra di dialogo campi. Successivamente, aggiungere un nuovo TemplateField e impostare relativi `HeaderText` proprietà su "Inventario prezzo /" e spostamento di TemplateField nuovo in modo che l'it è posizionata sopra il `UnitPrice` BoundField.


[![Aggiungere un nuovo TemplateField al controllo DetailsView](using-templatefields-in-the-detailsview-control-vb/_static/image11.png)](using-templatefields-in-the-detailsview-control-vb/_static/image10.png)

**Figura 4**: aggiungere un nuovo TemplateField al controllo DetailsView ([fare clic per visualizzare l'immagine con dimensioni normali](using-templatefields-in-the-detailsview-control-vb/_static/image12.png))


Poiché questo nuovo TemplateField conterrà i valori attualmente visualizzati nel `UnitPrice`, `UnitsInStock`, e `UnitsOnOrder` BoundField, verranno ora rimossi.

L'ultima attività per questo passaggio consiste nel definire il `ItemTemplate` markup per il prezzo e TemplateField inventario, che può essere eseguita tramite modifica interfaccia nella finestra di progettazione o manualmente tramite la sintassi dichiarativa del controllo del modello di DetailsView. Come per GridView, interfaccia di modifica modello di DetailsView sono accessibili facendo clic sul collegamento Modifica modelli nello smart tag. Da qui è possibile selezionare il modello da modificare nell'elenco a discesa e quindi aggiungere tutti i controlli Web dalla casella degli strumenti.

Per questa esercitazione, iniziare aggiungendo un controllo etichetta al prezzo e dell'inventario TemplateField `ItemTemplate`. Successivamente, fare clic sul collegamento Modifica DataBindings dallo smart tag del controllo Web Label e associare le `Text` proprietà per il `UnitPrice` campo.


[![Eseguire l'associazione di proprietà del testo dell'etichetta per il campo UnitPrice dati](using-templatefields-in-the-detailsview-control-vb/_static/image14.png)](using-templatefields-in-the-detailsview-control-vb/_static/image13.png)

**Figura 5**: eseguire l'associazione dell'etichetta `Text` proprietà per il `UnitPrice` campo dati ([fare clic per visualizzare l'immagine con dimensioni normali](using-templatefields-in-the-detailsview-control-vb/_static/image15.png))


## <a name="formatting-the-price-as-a-currency"></a>Il prezzo di formattazione come valuta

Grazie a questa aggiunta, il controllo Web Label prezzo e TemplateField inventario vengono ora visualizzati solo il prezzo per il prodotto selezionato. Figura 6 mostra una schermata di stato di avanzamento fino ad ora quando viene visualizzato tramite un browser.


[![Il prezzo e TemplateField inventario Mostra il costo](using-templatefields-in-the-detailsview-control-vb/_static/image17.png)](using-templatefields-in-the-detailsview-control-vb/_static/image16.png)

**Figura 6**: il prezzo e TemplateField inventario Mostra il costo ([fare clic per visualizzare l'immagine con dimensioni normali](using-templatefields-in-the-detailsview-control-vb/_static/image18.png))


Si noti che il prezzo del prodotto non è formattato come valuta. Con un BoundField, la formattazione è possibile impostando il `HtmlEncode` proprietà `False` e il `DataFormatString` proprietà `{0:formatSpecifier}`. Per un TemplateField, tuttavia, tutte le istruzioni di formattazione devono essere specificate nella sintassi di associazione dati o tramite l'uso di un metodo di formattazione definiti in un punto qualsiasi all'interno del codice dell'applicazione (ad esempio classe code-behind della pagina ASP.NET).

Per specificare la formattazione per la sintassi di associazione dati utilizzata nel controllo etichetta Web, tornare alla finestra di dialogo DataBindings facendo clic sul collegamento Modifica DataBindings dallo smart tag dell'etichetta. È possibile digitare le istruzioni di formattazione direttamente nella casella di riepilogo a discesa formato o selezionare una delle stringhe di formato definita. Come con i BoundField `DataFormatString` proprietà, la formattazione viene specificata usando `{0:formatSpecifier}`.

Per il `UnitPrice` utilizza la formattazione della valuta specificata selezionando il valore di elenco appropriato di elenco a discesa o digitando nel campo `{0:C}` manualmente.


[![Formattare il prezzo come valuta](using-templatefields-in-the-detailsview-control-vb/_static/image20.png)](using-templatefields-in-the-detailsview-control-vb/_static/image19.png)

**Figura 7**: il prezzo come una valuta di formato ([fare clic per visualizzare l'immagine con dimensioni normali](using-templatefields-in-the-detailsview-control-vb/_static/image21.png))


In modo dichiarativo, la formattazione specifica è indicata come secondo parametro nel `Bind` o `Eval` metodi. Le impostazioni effettuate solo tramite il risultato della finestra di progettazione nella seguente espressione di Data Binding nel markup dichiarativo:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample2.aspx)]

## <a name="adding-the-remaining-data-fields-to-the-templatefield"></a>Aggiunta di campi dati rimanenti al TemplateField

A questo punto abbiamo visualizzati e formattati i `UnitPrice` dati campo al prezzo e TemplateField inventario, ma è comunque necessario visualizzare il `UnitsInStock` e `UnitsOnOrder` campi. È possibile visualizzare questi su una riga sotto il prezzo e racchiuso tra parentesi. Nell'interfaccia di modifica modello nella finestra di progettazione, è possibile aggiungere tale markup si posiziona il cursore all'interno del modello e semplicemente digitando il testo da visualizzare. In alternativa, questo markup può essere immessi direttamente nella sintassi dichiarativa.

Aggiungere il markup statici, controlli Web di etichetta e la sintassi di associazione dati in modo che il prezzo e TemplateField inventario vengono visualizzate le informazioni di prezzo e inventario come segue:

*prezzo unitario*  
(**In magazzino o in ordine:** *UnitsInStock* / *UnitsOnOrder*)

Dopo aver eseguito questa attività markup dichiarativo di DetailsView dovrebbe essere simile al seguente:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample3.aspx)]

Con queste modifiche sono stati consolidati le informazioni di inventario e prezzo in una singola riga DetailsView.


[![Il prezzo e le informazioni di inventario vengono visualizzati in una riga singola](using-templatefields-in-the-detailsview-control-vb/_static/image23.png)](using-templatefields-in-the-detailsview-control-vb/_static/image22.png)

**Figura 8**: informazioni di inventario e il prezzo viene visualizzato in una riga singola ([fare clic per visualizzare l'immagine con dimensioni normali](using-templatefields-in-the-detailsview-control-vb/_static/image24.png))


## <a name="step-3-customizing-the-discontinued-field-information"></a>Passaggio 3: Personalizzare le informazioni di campo non più supportate

Il `Products` della tabella `Discontinued` colonna è un valore di bit che indica se il prodotto è stato sospeso. Quando si associa un controllo DetailsView (o GridView) a un controllo origine dati, ad esempio i campi, valore booleano `Discontinued`, vengono implementate come CheckBoxFields mentre i campi dei valori non booleani, ad esempio `ProductID`, `ProductName`e così via, vengono implementate come BoundField. Elemento CheckBoxField esegue il rendering come una casella di controllo disabilitata che viene controllata se il valore del campo dati è True e non controllato in caso contrario.

Invece di visualizzare il CampoCasellaDiControllo potremmo voler verrà invece visualizzato il testo che indica se il prodotto non è più disponibile. Per eseguire questa operazione fosse possibile rimuovere il CampoCasellaDiControllo da DetailsView e quindi aggiungere un BoundField cui `DataField` è stata impostata su `Discontinued`. Si consiglia di eseguire questa operazione. Dopo questa modifica DetailsView Mostra il testo "True" per i prodotti non più utilizzati e "False" per i prodotti che sono ancora attivi.


[![Le stringhe True e False sono utilizzate per visualizzare lo stato non più supportato](using-templatefields-in-the-detailsview-control-vb/_static/image26.png)](using-templatefields-in-the-detailsview-control-vb/_static/image25.png)

**Figura 9**: The stringhe True e False vengono usati per visualizzare lo stato sospeso ([fare clic per visualizzare l'immagine con dimensioni normali](using-templatefields-in-the-detailsview-control-vb/_static/image27.png))


Si supponga che volevamo le stringhe "True" o "False", ma "YES" e "NO", invece usare. Tale personalizzazione può essere eseguita con l'ausilio di un TemplateField e un metodo di formattazione. Un metodo di formattazione può richiedere un numero qualsiasi di parametri di input, ma deve restituire il codice HTML (sotto forma di stringa) per inserire il modello.

Aggiungere un metodo di formattazione per il `DetailsViewTemplateField.aspx` classe code-behind della pagina denominato `DisplayDiscontinuedAsYESorNO` che accetta un `Northwind.ProductsRow` oggetto come parametro di input e restituisce una stringa. Come illustrato nell'esercitazione precedente, questo metodo *devono* contrassegnato come `Protected` o `Public` perché sia accessibile dal modello.


[!code-vb[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample4.vb)]

Questo metodo controlla il parametro di input (`discontinued`) e restituisce "YES", se `True`"NO"; in caso contrario.

> [!NOTE]
> Nel metodo di formattazione esaminato nel richiamo dell'esercitazione precedente che si stava passando in un campo di dati che potrebbe contenere `NULL` s e, pertanto è necessario controllare se il dipendente `HiredDate` valore della proprietà disponesse di un database `NULL` valore prima l'accesso di `EmployeesRow`del `HiredDate` proprietà. Tale controllo non è necessario qui perché il `Discontinued` colonna non può mai essere database `NULL` valori assegnati. Inoltre, ecco perché il metodo può accettare un valore booleano di input parametro anziché dover accettare una `ProductsRow` istanza o un parametro di tipo `Object`.


Con questo metodo di formattazione completato, resta che chiamarla dal TemplateField `ItemTemplate`. Per creare il TemplateField rimuovere il `Discontinued` BoundField e aggiungere un nuovo TemplateField o convertire il `Discontinued` BoundField in un TemplateField. Quindi, dalla visualizzazione markup dichiarativo, modificare il TemplateField in modo che contenga solo un elemento ItemTemplate che richiama il `DisplayDiscontinuedAsYESorNO` , passando il valore dell'oggetto corrente `ProductRow` dell'istanza `Discontinued` proprietà. Ciò è possibile accedere tramite il `Eval` (metodo). In particolare, il markup del TemplateField dovrebbe essere simile:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample5.aspx)]

Ciò causerà il `DisplayDiscontinuedAsYESorNO` metodo da richiamare durante il rendering di DetailsView, passando il `ProductRow` dell'istanza `Discontinued` valore. Poiché il `Eval` metodo viene restituito un valore di tipo `Object`, ma la `DisplayDiscontinuedAsYESorNO` metodo prevede un parametro di input di tipo `Boolean`, verrà eseguito il cast di `Eval` metodi di un valore restituiscono per `Boolean`. Il `DisplayDiscontinuedAsYESorNO` metodo restituirà quindi "YES" o "NO" a seconda del valore ricevuto. Il valore restituito è ciò che viene visualizzato in questo controllo DetailsView di riga (vedere la figura 10).


[![I valori yes o NO sono ora visualizzati nella riga Discontinued](using-templatefields-in-the-detailsview-control-vb/_static/image29.png)](using-templatefields-in-the-detailsview-control-vb/_static/image28.png)

**Figura 10**: i valori YES o NO sono ora visualizzati nella riga Discontinued ([fare clic per visualizzare l'immagine con dimensioni normali](using-templatefields-in-the-detailsview-control-vb/_static/image30.png))


## <a name="summary"></a>Riepilogo

TemplateField nel controllo DetailsView consente un grado superiore di flessibilità per la visualizzazione dei dati è disponibile con gli altri controlli di campo e sono ideali per le situazioni in cui:

- Più campi dati devono essere visualizzati in una colonna GridView
- La data è espressa meglio tramite un controllo Web anziché di testo normale
- L'output varia a seconda dei dati sottostanti, ad esempio la visualizzazione dei metadati o in riformattazione dei dati

Anche se TemplateFields consentono un maggiore livello di flessibilità per il rendering di dati sottostanti di DetailsView, l'output di DetailsView ha ancora un po' squadrato come ogni campo viene visualizzato come una riga in un elemento HTML `<table>`.

Il controllo FormView offre un maggiore livello di flessibilità nella configurazione di output sottoposto a rendering. FormView non contiene campi, ma piuttosto soltanto una serie di modelli (`ItemTemplate`, `EditItemTemplate`, `HeaderTemplate`e così via). Si vedrà come usare FormView per ottenere un maggiore controllo del layout renderizzato nell'esercitazione successiva.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore capo di questa esercitazione è stata Dan Jagers. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](using-templatefields-in-the-gridview-control-vb.md)
> [Successivo](using-the-formview-s-templates-vb.md)
