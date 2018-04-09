---
uid: web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-detailsview-control-cs
title: Utilizzo di TemplateFields nel controllo DetailsView (c#) | Documenti Microsoft
author: rick-anderson
description: Le stesse funzionalità TemplateFields disponibili con GridView sono anche disponibili con il controllo DetailsView. In questa esercitazione, verrà visualizzato un prodotto...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 83efb21f-b231-446e-9356-f4c6cbcc6713
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-detailsview-control-cs
msc.type: authoredcontent
ms.openlocfilehash: f1d2e8312451c0bd1b3aba448963317f5fe06029
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="using-templatefields-in-the-detailsview-control-c"></a>Utilizzo di TemplateFields nel controllo DetailsView (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare App di esempio](http://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_13_CS.exe) o [Scarica il PDF](using-templatefields-in-the-detailsview-control-cs/_static/datatutorial13cs1.pdf)

> Le stesse funzionalità TemplateFields disponibili con GridView sono anche disponibili con il controllo DetailsView. In questa esercitazione, verrà visualizzato un prodotto alla volta utilizzando un controllo DetailsView contenente TemplateFields.


## <a name="introduction"></a>Introduzione

Il TemplateField offre maggiore flessibilità nel rendering dei dati del BoundField, CheckBoxField, HyperLinkField e di altri controlli di campo dati. Nel [esercitazione precedente](using-templatefields-in-the-gridview-control-cs.md) è descritto l'utilizzo di TemplateField in GridView per:

- Visualizzare più valori di campo di dati in una colonna. In particolare, entrambi i `FirstName` e `LastName` campi sono stati combinati in una colonna di GridView.
- Usare un controllo Web alternativo per esprimere un valore del campo dati. È stato illustrato come visualizzare il `HiredDate` valore utilizzando un controllo di calendario.
- Visualizza le informazioni di stato in base ai dati sottostanti. Mentre il `Employees` tabella non contiene una colonna che restituisce il numero di giorni di un dipendente è stato del processo, è possibile visualizzare tali informazioni nell'esempio di GridView nell'esercitazione precedente con l'utilizzo di un TemplateField e metodo di formattazione.

Le stesse funzionalità TemplateFields disponibili con GridView sono anche disponibili con il controllo DetailsView. In questa esercitazione, verrà visualizzato un prodotto alla volta utilizzando un controllo DetailsView che contiene due TemplateFields. Il primo TemplateField combinerà i `UnitPrice`, `UnitsInStock`, e `UnitsOnOrder` campi di dati in un'unica riga di DetailsView. Il secondo TemplateField visualizzerà il valore di `Discontinued` campo, ma verrà utilizzato un metodo di formattazione per visualizzare "YES" se `Discontinued` è `true`e "NO", in caso contrario.


[![Due TemplateFields vengono utilizzati per personalizzare la visualizzazione](using-templatefields-in-the-detailsview-control-cs/_static/image2.png)](using-templatefields-in-the-detailsview-control-cs/_static/image1.png)

**Figura 1**: due TemplateFields vengono utilizzati per personalizzare la visualizzazione ([fare clic per visualizzare l'immagine ingrandita](using-templatefields-in-the-detailsview-control-cs/_static/image3.png))


Iniziamo!

## <a name="step-1-binding-the-data-to-the-detailsview"></a>Passaggio 1: Associazione di dati al controllo DetailsView.

Come descritto nell'esercitazione precedente, quando si lavora con TemplateFields è spesso più semplice iniziare creando il controllo DetailsView che contiene solo BoundField e aggiungere di nuovo TemplateFields o convertire il BoundField esistente TemplateFields come necessario. . Pertanto, è possibile iniziare questa esercitazione, aggiunta di un controllo DetailsView alla pagina tramite la finestra di progettazione e associarla a ObjectDataSource che restituisce l'elenco dei prodotti. Questi passaggi verranno creato un controllo DetailsView con BoundField per ognuno dei campi dei valori non booleani del prodotto e un CheckBoxField per quel campo valore booleano (sospeso).

Aprire il `DetailsViewTemplateField.aspx` pagina e trascinare un controllo DetailsView dalla casella degli strumenti nella finestra di progettazione. Smart tag del controllo DetailsView scegliere di aggiungere un nuovo controllo ObjectDataSource che richiama la `ProductsBLL` della classe `GetProducts()` metodo.


[![Aggiungere un nuovo controllo ObjectDataSource che richiama il metodo GetProducts()](using-templatefields-in-the-detailsview-control-cs/_static/image5.png)](using-templatefields-in-the-detailsview-control-cs/_static/image4.png)

**Figura 2**: aggiungere un nuovo controllo ObjectDataSource tale Invoke il `GetProducts()` metodo ([fare clic per visualizzare l'immagine ingrandita](using-templatefields-in-the-detailsview-control-cs/_static/image6.png))


Per questo report è possibile rimuovere il `ProductID`, `SupplierID`, `CategoryID`, e `ReorderLevel` BoundField. Successivamente, riordinare il BoundField in modo che il `CategoryName` e `SupplierName` BoundField vengono visualizzati immediatamente dopo il `ProductName` BoundField. È possibile regolare la `HeaderText` proprietà e le proprietà di formattazione per il BoundField come si ritengono appropriato. Come in GridView, queste modifiche BoundField livello possono essere eseguite tramite la finestra di dialogo campi (accessibile facendo clic sul collegamento Modifica campi nello smart tag del controllo DetailsView.) o tramite la sintassi dichiarativa. Infine, cancellare il DetailsView `Height` e `Width` i valori delle proprietà per consentire il controllo DetailsView controllare per espandere i dati visualizzati in base e selezionare la casella di controllo Abilita Paging nello smart tag.

Dopo aver apportato queste modifiche, markup dichiarativo del controllo DetailsView dovrebbe essere simile al seguente:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample1.aspx)]

Richiedere qualche istante per visualizzare la pagina tramite un browser. A questo punto dovrebbe essere un singolo prodotto elencato (Chai) con le righe che mostra il nome del prodotto, categoria, fornitore, prezzo, unità in magazzino, quantità ordinata e il relativo stato non più supportato.


[![I dettagli del prodotto vengono visualizzati tramite una serie di BoundField](using-templatefields-in-the-detailsview-control-cs/_static/image8.png)](using-templatefields-in-the-detailsview-control-cs/_static/image7.png)

**Figura 3**: sono riportati i dettagli del prodotto di utilizzando una serie di BoundField ([fare clic per visualizzare l'immagine ingrandita](using-templatefields-in-the-detailsview-control-cs/_static/image9.png))


## <a name="step-2-combining-the-price-units-in-stock-and-units-on-order-into-one-row"></a>Passaggio 2: La combinazione della prezzo, unità In magazzino e le unità nell'ordine in una riga

DetailsView con una riga per il `UnitPrice`, `UnitsInStock`, e `UnitsOnOrder` campi. È possibile combinare questi campi di dati in una sola riga con un TemplateField aggiungendo un nuovo TemplateField o convertendo uno esistente `UnitPrice`, `UnitsInStock`, e `UnitsOnOrder` BoundField in un TemplateField. Mentre preferisca conversione BoundField esistente, si esercitazione aggiungendo un nuovo TemplateField.

Avviare facendo clic sul collegamento Modifica campi nello smart tag di DetailsView viene visualizzata la finestra di dialogo campi. Successivamente, aggiungere un nuovo TemplateField e impostare il relativo `HeaderText` proprietà su "Inventario prezzo /" e spostare la nuova TemplateField in modo che non è posizionato sopra il `UnitPrice` BoundField.


[![Aggiungere un nuovo TemplateField al controllo DetailsView](using-templatefields-in-the-detailsview-control-cs/_static/image11.png)](using-templatefields-in-the-detailsview-control-cs/_static/image10.png)

**Figura 4**: aggiungere un nuovo TemplateField al controllo DetailsView ([fare clic per visualizzare l'immagine ingrandita](using-templatefields-in-the-detailsview-control-cs/_static/image12.png))


Poiché questo nuovo TemplateField conterrà i valori attualmente visualizzati nel `UnitPrice`, `UnitsInStock`, e `UnitsOnOrder` BoundField, consente di rimuoverli.

L'ultima attività per questo passaggio consiste nel definire il `ItemTemplate` markup per il prezzo e TemplateField inventario, che può essere eseguita tramite il modello di DetailsView modifica interfaccia nella finestra di progettazione o manualmente tramite la sintassi dichiarativa del controllo. Come in GridView, interfaccia di modifica modello di DetailsView è accessibile facendo clic sul collegamento Modifica modelli nello smart tag. Da qui è possibile selezionare il modello da modificare dall'elenco a discesa e quindi aggiungere tutti i controlli Web dalla casella degli strumenti.

Per questa esercitazione, per iniziare, aggiungere un controllo etichetta per il prezzo e dell'inventario TemplateField `ItemTemplate`. Successivamente, fare clic sul collegamento Modifica DataBindings di smart tag del controllo etichetta Web e associare il `Text` proprietà per il `UnitPrice` campo.


[![Associare la proprietà di testo dell'etichetta per il campo dei dati UnitPrice](using-templatefields-in-the-detailsview-control-cs/_static/image14.png)](using-templatefields-in-the-detailsview-control-cs/_static/image13.png)

**Figura 5**: associare l'etichetta `Text` proprietà per il `UnitPrice` campo dei dati ([fare clic per visualizzare l'immagine ingrandita](using-templatefields-in-the-detailsview-control-cs/_static/image15.png))


## <a name="formatting-the-price-as-a-currency"></a>Il prezzo di formattazione come valuta

Con l'aggiunta, il controllo Web Label prezzo e TemplateField inventario visualizzerà solo il prezzo del prodotto selezionato. Figura 6 mostra una schermata dello stato di avanzamento finora quando viene visualizzato tramite un browser.


[![Il prezzo e TemplateField inventario Mostra il prezzo](using-templatefields-in-the-detailsview-control-cs/_static/image17.png)](using-templatefields-in-the-detailsview-control-cs/_static/image16.png)

**Figura 6**: il prezzo e TemplateField inventario Mostra il prezzo ([fare clic per visualizzare l'immagine ingrandita](using-templatefields-in-the-detailsview-control-cs/_static/image18.png))


Si noti che il prezzo del prodotto non è formattato come valuta. Con un BoundField, la formattazione è possibile impostare il `HtmlEncode` proprietà `false` e `DataFormatString` proprietà `{0:formatSpecifier}`. Per un TemplateField, tuttavia, le istruzioni di formattazione devono specificare la sintassi di associazione dati o tramite l'utilizzo di un metodo di formattazione definiti in un punto qualsiasi all'interno del codice dell'applicazione (ad esempio una classe code-behind della pagina ASP.NET).

Per specificare la formattazione per la sintassi di associazione dati utilizzata per il controllo etichetta Web, tornare nella finestra di dialogo DataBindings facendo clic sul collegamento Modifica DataBindings smart tag dell'etichetta. È possibile digitare le istruzioni di formattazione direttamente nella casella di riepilogo a discesa formato o selezionare una delle stringhe di formato definita. Come con i BoundField `DataFormatString` proprietà, la formattazione è specificata utilizzando `{0:formatSpecifier}`.

Per il `UnitPrice` utilizzare il formato valuta specificato selezionando il valore di elenco a discesa appropriato o digitando nel campo `{0:C}` manualmente.


[![Formattare il prezzo come valuta](using-templatefields-in-the-detailsview-control-cs/_static/image20.png)](using-templatefields-in-the-detailsview-control-cs/_static/image19.png)

**Figura 7**: il prezzo come valuta di formato ([fare clic per visualizzare l'immagine ingrandita](using-templatefields-in-the-detailsview-control-cs/_static/image21.png))


In modo dichiarativo, la formattazione specifica è indicata come secondo parametro nel `Bind` o `Eval` metodi. Le impostazioni effettuate solo tramite il risultato della finestra di progettazione nell'espressione di associazione dati seguenti nel markup dichiarativo:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample2.aspx)]

## <a name="adding-the-remaining-data-fields-to-the-templatefield"></a>Aggiungere i campi dati rimanenti la TemplateField

A questo punto è stato visualizzato e formattato il `UnitPrice` dati campo al prezzo e TemplateField inventario, ma è necessario visualizzare il `UnitsInStock` e `UnitsOnOrder` campi. Verranno visualizzati in una linea di sotto di prezzo e racchiuso tra parentesi. Da interfaccia di modifica di modello nella finestra di progettazione, è possibile aggiungere tale markup posizionando il cursore all'interno del modello e digitare semplicemente il testo da visualizzare. In alternativa, questo markup può essere immessi direttamente nella sintassi dichiarativa.

Aggiungere il markup statico, i controlli Web di etichetta e sintassi di associazione dati in modo che il prezzo e TemplateField inventario vengono visualizzate le informazioni di prezzo e inventario come segue:

*UnitPrice*  
(**In magazzino / ordine:** *UnitsInStock* / *UnitsOnOrder*)

Dopo l'esecuzione di questa attività markup dichiarativo di DetailsView dovrebbe essere simile al seguente:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample3.aspx)]

Con queste modifiche è stato consolidato le informazioni di prezzo e inventario in una sola riga di DetailsView.


[![I prezzi e le informazioni di inventario vengano visualizzati in una singola riga](using-templatefields-in-the-detailsview-control-cs/_static/image23.png)](using-templatefields-in-the-detailsview-control-cs/_static/image22.png)

**Figura 8**: le informazioni di inventario e il prezzo viene visualizzato in una riga singola ([fare clic per visualizzare l'immagine ingrandita](using-templatefields-in-the-detailsview-control-cs/_static/image24.png))


## <a name="step-3-customizing-the-discontinued-field-information"></a>Passaggio 3: Personalizzazione delle informazioni del campo non più supportate

Il `Products` della tabella `Discontinued` colonna è un valore di bit che indica se il prodotto è stato sospeso. Quando si associa un controllo DetailsView GridView (o) a un controllo origine dati, ad esempio i campi di un valore booleano, `Discontinued`, vengono implementate come CheckBoxFields mentre campi dei valori non booleani, ad esempio `ProductID`, `ProductName`e così via, vengono implementate come BoundField. Elemento CheckBoxField esegue il rendering come una casella di controllo disabilitata se il valore del campo dati è True e non è selezionata.

Anziché visualizzare l'elemento CheckBoxField potrebbe voler invece visualizzato il testo che indica se il prodotto non è più disponibile. A tale scopo è stato possibile rimuovere l'elemento CheckBoxField dal controllo DetailsView e quindi aggiungere un BoundField cui `DataField` è stata impostata su `Discontinued`. Richiedere qualche istante per eseguire questa operazione. Dopo questa modifica DetailsView viene visualizzato il testo "True" per i prodotti non più supportati e "False" per i prodotti che sono ancora attivi.


[![Le stringhe True e False sono utilizzate per visualizzare lo stato non più supportato](using-templatefields-in-the-detailsview-control-cs/_static/image26.png)](using-templatefields-in-the-detailsview-control-cs/_static/image25.png)

**Figura 9**: il stringhe True e False vengono utilizzati per visualizzare lo stato sospeso ([fare clic per visualizzare l'immagine ingrandita](using-templatefields-in-the-detailsview-control-cs/_static/image27.png))


Si supponga non vogliamo le stringhe utilizzato, ma "YES" e "NO", "True" o "False". Queste attività di personalizzazione possono essere eseguite con l'aiuto di un TemplateField e un metodo di formattazione. Un metodo di formattazione può richiedere un numero qualsiasi di parametri di input, ma deve restituire il codice HTML (come una stringa) per inserire nel modello.

Aggiungere un metodo di formattazione per il `DetailsViewTemplateField.aspx` classe code-behind della pagina denominata `DisplayDiscontinuedAsYESorNO` che accetta un valore booleano come parametro di input e restituisce una stringa. Come descritto nell'esercitazione precedente, questo metodo *deve* essere contrassegnato come `protected` o `public` per essere accessibili dal modello.


[!code-csharp[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample4.cs)]

Questo metodo controlla il parametro di input (`discontinued`) e restituisce "YES" se è `true`"NO", in caso contrario.

> [!NOTE]
> In metodo di formattazione esaminato il richiamo di esercitazione precedente che si stava passando in un campo di dati che potrebbe contenere `NULL` s e, pertanto è necessario verificare se il dipendente `HiredDate` valore della proprietà aveva un database `NULL` valore prima di l'accesso di `EmployeesRow`del `HiredDate` proprietà. Tale controllo non è necessario qui perché il `Discontinued` colonna non può mai essere database `NULL` valori assegnati. Inoltre, è per questo motivo il metodo può accettare un valore booleano di input parametro anziché dover accettare un `ProductsRow` istanza o un parametro di tipo `object`.


Con questo metodo di formattazione completato, è comunque chiamarlo dal TemplateField `ItemTemplate`. Per creare il TemplateField rimuovere il `Discontinued` BoundField e aggiungere un nuovo TemplateField o convertire il `Discontinued` BoundField in un TemplateField. Quindi, dalla visualizzazione markup dichiarativo, modificare il TemplateField in modo che contenga solo un elemento ItemTemplate che richiama la `DisplayDiscontinuedAsYESorNO` , passando il valore dell'oggetto corrente `ProductRow` dell'istanza `Discontinued` proprietà. Questo è possibile accedere tramite il `Eval` metodo. In particolare, il markup del TemplateField dovrebbe essere simile:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample5.aspx)]

In questo modo il `DisplayDiscontinuedAsYESorNO` metodo da richiamare quando il rendering di DetailsView, passando il `ProductRow` dell'istanza `Discontinued` valore. Poiché il `Eval` il metodo restituisce un valore di tipo `object`, ma la `DisplayDiscontinuedAsYESorNO` metodo prevede un parametro di input di tipo `bool`, verrà eseguito il cast di `Eval` metodi di un valore restituiscono per `bool`. Il `DisplayDiscontinuedAsYESorNO` metodo restituirà quindi "YES" o "NO" a seconda del valore riceve. Il valore restituito è contenuto in questo controllo DetailsView riga (vedere la figura 10).


[![I valori yes o NO sono ora visualizzati nella riga Discontinued](using-templatefields-in-the-detailsview-control-cs/_static/image29.png)](using-templatefields-in-the-detailsview-control-cs/_static/image28.png)

**Figura 10**: i valori YES o NO sono ora visualizzati nella riga Discontinued ([fare clic per visualizzare l'immagine ingrandita](using-templatefields-in-the-detailsview-control-cs/_static/image30.png))


## <a name="summary"></a>Riepilogo

TemplateField nel controllo DetailsView consente un maggiore livello di flessibilità per la visualizzazione di dati che è disponibile con gli altri controlli di campo e sono ideali per le situazioni in cui:

- Più campi dati devono essere visualizzate in una colonna di GridView
- La data è espressa meglio con un controllo Web anziché come testo normale
- L'output dipende dai dati sottostanti, ad esempio la visualizzazione dei metadati o nelle riformattando i dati

Mentre per un maggiore livello di flessibilità per il rendering dei dati sottostanti di DetailsView TemplateFields, l'output di DetailsView abbia ancora un po' squadrato come ciascun campo viene visualizzato come una riga in un elemento HTML `<table>`.

Il controllo FormView offre un maggiore livello di flessibilità nella configurazione di output sottoposto a rendering. FormView non contiene campi, ma semplicemente una serie di modelli (`ItemTemplate`, `EditItemTemplate`, `HeaderTemplate`e così via). Vedremo come utilizzare il controllo FormView per ottenere un maggiore controllo del layout del rendering nell'esercitazione successiva.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore per questa esercitazione è stata Dan Jagers. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](using-templatefields-in-the-gridview-control-cs.md)
> [Successivo](using-the-formview-s-templates-cs.md)
