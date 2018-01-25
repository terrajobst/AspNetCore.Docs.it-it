---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb
title: Aggiunta di controlli di convalida per la modifica e l'inserimento di interfacce (VB) | Documenti Microsoft
author: rick-anderson
description: "In questa esercitazione si vedrà quanto sia facile aggiungere i controlli di convalida EditItemTemplate e InsertItemTemplate dei dati di controllo Web, per offrire maggiori..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: e3d7028a-7a22-4a4f-babe-d53afc41c0e2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb
msc.type: authoredcontent
ms.openlocfilehash: 53414aa17514d07083fe05b8c2abcba10a01cf98
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="adding-validation-controls-to-the-editing-and-inserting-interfaces-vb"></a>Aggiunta di controlli di convalida per la modifica e l'inserimento di interfacce (Visual Basic)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare App di esempio](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_19_VB.exe) o [Scarica il PDF](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/datatutorial19vb1.pdf)

> In questa esercitazione verranno esaminati quanto sia facile aggiungere i controlli di convalida EditItemTemplate e InsertItemTemplate dei dati di controllo Web, per fornire un'interfaccia utente più infallibile.


## <a name="introduction"></a>Introduzione

I controlli GridView e DetailsView negli esempi abbiamo esplorato negli ultimi tre esercitazioni sono tutti stato composto da BoundField e CheckBoxFields (i tipi di campo aggiunti automaticamente da Visual Studio quando si associa un controllo GridView o DetailsView a un'origine dati controllo tramite lo smart tag). Quando si modifica una riga in un controllo GridView o DetailsView, tali BoundField che non sono di sola lettura vengono convertiti nelle caselle di testo, da cui l'utente finale può modificare i dati esistenti. Analogamente, quando inserire un nuovo record in un controllo DetailsView, tali BoundField cui `InsertVisible` è impostata su `True` (predefinito) vengono visualizzati come caselle di testo vuota, in cui l'utente può fornire i valori dei campi del nuovo record. Analogamente, CheckBoxFields, che sono disabilitate nell'interfaccia standard, di sola lettura, vengono convertiti in caselle di controllo abilitato la modifica e l'inserimento di interfacce.

Durante l'impostazione predefinita, la modifica e l'inserimento di interfacce per il BoundField e CheckBoxField può essere utile, l'interfaccia non dispone di alcun tipo di convalida. Se un utente esegue un errore di immissione dati - ad esempio l'omissione di `ProductName` campo oppure immettendo un valore valido per `UnitsInStock` (ad esempio -50) verrà generata un'eccezione da all'interno di efficacia dell'architettura dell'applicazione. Durante questa eccezione può essere gestita correttamente come dimostrato nel [esercitazione precedente](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md), idealmente la modifica o l'inserimento dell'interfaccia utente includono i controlli di convalida per impedire un utente di immettere tali dati non validi nel primo elemento.

Per fornire una modalità di modifica personalizzata o un'interfaccia di inserimento, è necessario sostituire il BoundField o CheckBoxField con un TemplateField. TemplateFields, ovvero l'argomento di discussione nel [TemplateFields utilizzo nel controllo GridView](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) e [TemplateFields utilizzo nel controllo DetailsView](../custom-formatting/using-templatefields-in-the-detailsview-control-vb.md) esercitazioni, può essere costituito da più la definizione di modelli di separano le interfacce per gli stati di riga diversi. Il TemplateField `ItemTemplate` viene utilizzato per il rendering di campi di sola lettura o le righe nel controllo DetailsView o GridView, mentre il `EditItemTemplate` e `InsertItemTemplate` indicano le interfacce da utilizzare per la modifica e l'inserimento di modalità, rispettivamente.

In questa esercitazione si vedrà quanto sia facile aggiungere i controlli di convalida per il TemplateField `EditItemTemplate` e `InsertItemTemplate` per fornire un'interfaccia utente più infallibile. In particolare, questa esercitazione illustra l'esempio creato nel [esaminando gli eventi associati con l'inserimento, aggiornamento ed eliminazione](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md) esercitazione e consente la modifica e l'inserimento delle interfacce per includere la convalida appropriata.

## <a name="step-1-replicating-the-example-fromexamining-the-events-associated-with-inserting-updating-and-deletingexamining-the-events-associated-with-inserting-updating-and-deleting-vbmd"></a>Passaggio 1: La replica di esempio da[esaminando gli eventi associati di inserimento, aggiornamento ed eliminazione](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)

Nel [esaminando gli eventi associati con l'inserimento, aggiornamento ed eliminazione](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md) esercitazione è stata creata una pagina elencati i nomi e i prezzi dei prodotti in un controllo GridView. modificabile. Inoltre, la pagina è incluso un controllo DetailsView cui `DefaultMode` è stata impostata su `Insert`, pertanto è sempre il rendering in modalità di inserimento. Da questo controllo DetailsView, l'utente può immettere il nome e il prezzo per un nuovo prodotto, fare clic su Inserisci e aggiungerlo al sistema (vedere la figura 1).


[![Nell'esempio precedente consente agli utenti di aggiungere nuovi prodotti e modificare quelli esistenti](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image2.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image1.png)

**Figura 1**: il precedente esempio consente agli utenti di aggiungere nuovi prodotti e modificare quelli esistenti ([fare clic per visualizzare l'immagine ingrandita](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image3.png))


L'obiettivo di questa esercitazione è per aumentare il controllo DetailsView e GridView per fornire i controlli di convalida. In particolare, la logica di convalida sarà:

- Richiede che il nome deve essere fornito durante l'inserimento o modifica di un prodotto
- Richiede che il prezzo deve essere fornito quando si inserisce un record. Quando si modifica un record, si sarà comunque necessario un prezzo, ma utilizzerà la logica di programmazione in GridView `RowUpdating` gestore dell'evento dall'esercitazione precedente è già presente
- Verificare che il valore immesso per il prezzo è un formato di valuta validi

Prima di poter esaminare in modo da integrare l'esempio precedente per includere la convalida, è innanzitutto necessario replicare l'esempio dal `DataModificationEvents.aspx` pagina per la pagina per questa esercitazione, `UIValidation.aspx`. A tale scopo è necessario copiare sia la `DataModificationEvents.aspx` markup dichiarativo della pagina e il relativo codice sorgente. Copiare innanzitutto il markup dichiarativo eseguendo i passaggi seguenti:

1. Aprire il `DataModificationEvents.aspx` pagina in Visual Studio
2. Passare al codice dichiarativo della pagina (fare clic sul pulsante nella parte inferiore della pagina di origine)
3. Copiare il testo all'interno di `<asp:Content>` e `</asp:Content>` tag (righe 3 a 44), come illustrato nella figura 2.


[![Copiare il testo all'interno del &lt;asp: Content&gt; controllo](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image5.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image4.png)

**Figura 2**: copiare il testo all'interno del `<asp:Content>` controllo ([fare clic per visualizzare l'immagine ingrandita](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image6.png))


1. Aprire il `UIValidation.aspx` pagina
2. Passare al codice dichiarativo della pagina
3. Incollare il testo all'interno di `<asp:Content>` controllo.

Per copiare il codice sorgente, aprire il `DataModificationEvents.aspx.vb` pagina e copiare solo il testo *all'interno di* la `EditInsertDelete_DataModificationEvents` classe. Copiare i tre gestori eventi (`Page_Load`, `GridView1_RowUpdating`, e `ObjectDataSource1_Inserting`), ma **non** copiare la dichiarazione di classe. Incollare il testo copiato *all'interno di* il `EditInsertDelete_UIValidation` classe `UIValidation.aspx.vb`.

Dopo avere spostato il contenuto e il codice da `DataModificationEvents.aspx` a `UIValidation.aspx`, dedicare alcuni minuti per il test dello stato di avanzamento in un browser. Si noterà lo stesso output e la stessa funzionalità in ognuna di queste due pagine esperienza (indietro per vedere la figura 1 una cattura di schermata della `DataModificationEvents.aspx` in azione).

## <a name="step-2-converting-the-boundfields-into-templatefields"></a>Passaggio 2: Conversione di BoundField in TemplateFields

Per aggiungere i controlli di convalida per le interfacce di modificare e l'inserimento, devono essere convertiti in TemplateFields il BoundField usato dai controlli DetailsView e GridView. A tale scopo, scegliere i collegamenti di modifica colonne e modificare i campi in GridView e DetailsView smart tag, rispettivamente. Qui, selezionare ogni il BoundField e fare clic sul collegamento "Converti il campo in un TemplateField".


[![Convertire ogni BoundField di DetailsView e GridView in TemplateFields](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image8.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image7.png)

**Figura 3**: convertirli singolarmente di DetailsView e GridView BoundField in TemplateFields ([fare clic per visualizzare l'immagine ingrandita](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image9.png))


Conversione di un BoundField in un TemplateField tramite la finestra di dialogo campi genera un TemplateField che presenta le stesse interfacce di sola lettura, modificare e inserimento BoundField stesso. Il markup seguente mostra la sintassi dichiarativa per il `ProductName` campo nel controllo DetailsView. dopo aver convertito in un TemplateField:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample1.aspx)]

Si noti che questo TemplateField ha tre modelli creati automaticamente `ItemTemplate`, `EditItemTemplate`, e `InsertItemTemplate`. Il `ItemTemplate` consente di visualizzare il valore di un campo di dati single (`ProductName`) utilizza un controllo etichetta Web, mentre il `EditItemTemplate` e `InsertItemTemplate` presentare il valore del campo dati in un controllo casella di testo Web che associa il campo dei dati con la casella di testo `Text` proprietà con associazione a dati bidirezionale. Poiché il controllo DetailsView vengono utilizzate solo in questa pagina per l'inserimento, è possibile rimuovere il `ItemTemplate` e `EditItemTemplate` da due TemplateFields, sebbene non sia lasciarli non causa problemi.

Poiché GridView non supporta l'inserimento di funzionalità del controllo DetailsView, conversione di GridView predefinito `ProductName` campo in un TemplateField comporta solo un `ItemTemplate` e `EditItemTemplate`:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample2.aspx)]

Fare clic su "Converti il campo in un TemplateField", Visual Studio ha creato un TemplateField cui modelli di simulano l'interfaccia utente di BoundField convertito. È possibile verificare questa, visitare questa pagina tramite un browser. Sono disponibili che l'aspetto e il comportamento del TemplateFields è identico all'esperienza quando sono stati utilizzati BoundField.

> [!NOTE]
> È possibile personalizzare le interfacce modificare nei modelli in base alle esigenze. Potrebbe ad esempio, si vuole avere la casella di testo nel `UnitPrice` TemplateFields sottoposto a rendering come una casella di testo più piccola rispetto di `ProductName` casella di testo. A tale scopo è possibile impostare la casella di testo `Columns` proprietà su un valore appropriato oppure specificare una larghezza assoluta tramite il `Width` proprietà. Nella prossima esercitazione vedremo come personalizzare completamente l'interfaccia di modifica sostituendo la casella di testo con una voce di dati alternativi controllo Web.


## <a name="step-3-adding-the-validation-controls-to-the-gridviewsedititemtemplate-s"></a>Passaggio 3: Aggiunta di controlli di convalida per il controllo GridView`EditItemTemplate` s

Quando si crea il form di immissione dati, è importante che gli utenti immettere tutti i campi obbligatori e che tutti gli input forniti sono valori validi, formattato correttamente. Per garantire che l'input dell'utente siano valide, ASP.NET fornisce cinque i controlli di convalida incorporate che sono progettati per essere utilizzati per convalidare il valore di un singolo controllo di input:

- [RequiredFieldValidator](https://msdn.microsoft.com/library/5hbw267h(VS.80).aspx) assicura che è stato fornito un valore
- [CompareValidator](https://msdn.microsoft.com/library/db330ayw(VS.80).aspx) convalida di un valore rispetto a un altro valore di controllo Web o un valore costante o assicura che il formato del valore è valido per un tipo di dati specificato
- [RangeValidator](https://msdn.microsoft.com/library/f70d09xt.aspx) garantisce che un valore all'interno di un intervallo di valori
- [RegularExpressionValidator](https://msdn.microsoft.com/library/eahwtc9e.aspx) convalida di un valore rispetto a un [espressioni regolari](http://en.wikipedia.org/wiki/Regular_expression)
- [CustomValidator](https://msdn.microsoft.com/library/9eee01cx(VS.80).aspx) convalida di un valore rispetto a un metodo personalizzato definito dall'utente

Per ulteriori informazioni su questi cinque controlli, consultare il [sezione relativa ai controlli di convalida](https://quickstarts.asp.net/quickstartv20/aspnet/doc/ctrlref/validation/default.aspx) del [esercitazioni delle Guide rapide ASP.NET](https://asp.net/QuickStart/aspnet/).

Per questa esercitazione è necessario utilizzare un RequiredFieldValidator DetailsView sia GridView in `ProductName` TemplateFields e RequiredFieldValidator dal controllo DetailsView `UnitPrice` TemplateField. Inoltre, sarà necessario aggiungere un controllo CompareValidator per entrambi i controlli `UnitPrice` TemplateFields che assicura che il prezzo immesso con un valore maggiore o uguale a 0 e verrà presentato in un formato di valuta validi.

> [!NOTE]
> Mentre ASP.NET 1. x era questi stessi controlli di convalida di cinque, ASP.NET 2.0 ha aggiunto una serie di miglioramenti, principale due script sul lato client da supportare per browser diverso da Internet Explorer e la possibilità di controlli di convalida di partizione in una pagina in gruppi di convalida. Per ulteriori informazioni sulle nuove funzionalità di controllo di convalida in 2.0, fare riferimento a [Sezionando i controlli di convalida in ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx).


Per iniziare, aggiungere i controlli di convalida necessari per il `EditItemTemplate` s in TemplateFields del controllo GridView. A tale scopo, fare clic sul collegamento Modifica modelli dal smart tag di GridView per visualizzare l'interfaccia di modifica del modello. Da qui è possibile selezionare il modello da modificare dall'elenco a discesa. Poiché si desidera estendere l'interfaccia di modifica, è necessario aggiungere i controlli di convalida per il `ProductName` e `UnitPrice`del `EditItemTemplate` s.


[![È necessario estendere il ProductName ed EditItemTemplates del prezzo unitario](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image11.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image10.png)

**Figura 4**: È necessario estendere il `ProductName` e `UnitPrice`del `EditItemTemplate` s ([fare clic per visualizzare l'immagine ingrandita](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image12.png))


Nel `ProductName` `EditItemTemplate`, aggiungere un controllo RequiredFieldValidator trascinandolo dalla casella degli strumenti all'interfaccia di modifica del modello, inserire dopo la casella di testo.


[![Aggiungere un controllo RequiredFieldValidator ProductName EditItemTemplate](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image14.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image13.png)

**Figura 5**: aggiungere un controllo RequiredFieldValidator per il `ProductName` `EditItemTemplate` ([fare clic per visualizzare l'immagine ingrandita](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image15.png))


Tutti i controlli di convalida di lavoro tramite la convalida dell'input di un singolo controllo Web ASP.NET. Pertanto, è necessario indicare che è stato appena aggiunto RequiredFieldValidator deve convalidare contro la casella di testo nel `EditItemTemplate`; questa operazione viene eseguita mediante l'impostazione del controllo di convalida [proprietà ControlToValidate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.controltovalidate(VS.80).aspx) per il `ID` del controllo Web appropriato. La casella di testo è attualmente il piuttosto nondescript `ID` di `TextBox1`, ma di seguito in un nome più appropriato. Fare clic sulla casella di testo nel modello e dalla finestra delle proprietà, modificare quindi il `ID` da `TextBox1` a `EditProductName`.


[![Modificare ID la casella di testo in EditProductName](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image17.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image16.png)

**Figura 6**: modificare la casella di testo `ID` a `EditProductName` ([fare clic per visualizzare l'immagine ingrandita](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image18.png))


Successivamente, impostare il controllo RequiredFieldValidator `ControlToValidate` proprietà `EditProductName`. Infine, impostare il [proprietà ErrorMessage](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.errormessage(VS.80).aspx) "È necessario fornire il nome del prodotto" e [proprietà Text](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.text(VS.80).aspx) per "\*". Il `Text` valore della proprietà, se fornito, è il testo visualizzato dal controllo di convalida se la convalida non riesce. Il `ErrorMessage` valore della proprietà, è necessario, viene utilizzato il controllo ValidationSummary; se il `Text` valore della proprietà viene omessa, il `ErrorMessage` valore della proprietà è anche il testo visualizzato dal controllo di convalida un input non valido.

Dopo l'impostazione di queste tre proprietà di RequiredFieldValidator la schermata dovrebbe essere simile alla figura 7.


[![Impostare il controllo RequiredFieldValidator ControlToValidate ErrorMessage e proprietà di testo](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image20.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image19.png)

**Figura 7**: impostare il controllo RequiredFieldValidator `ControlToValidate`, `ErrorMessage`, e `Text` proprietà ([fare clic per visualizzare l'immagine ingrandita](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image21.png))


Con RequiredFieldValidator aggiunti il `ProductName` `EditItemTemplate`, che rimane consiste nell'aggiungere la convalida necessaria per tutti i `UnitPrice` `EditItemTemplate`. Poiché per questa pagina, abbiamo deciso che, di `UnitPrice` è facoltativo quando modifica un record, non è necessario aggiungere un controllo RequiredFieldValidator. Tuttavia, è necessario aggiungere un controllo CompareValidator per assicurarsi che il `UnitPrice`, se fornito, viene correttamente formattati come valuta ed è maggiore o uguale a 0.

Prima di aggiungere il controllo CompareValidator per il `UnitPrice` `EditItemTemplate`, è necessario innanzitutto modificare ID del controllo TextBox Web da `TextBox2` a `EditUnitPrice`. Dopo aver apportato questa modifica, aggiungere il controllo CompareValidator, l'impostazione relativa `ControlToValidate` proprietà `EditUnitPrice`, le `ErrorMessage` proprietà su "il prezzo deve essere maggiore o uguale a zero e non può includere il simbolo di valuta," e il relativo `Text` proprietà "\*".

Per indicare che il `UnitPrice` valore deve essere maggiore o uguale a 0, impostare il controllo CompareValidator [proprietà Operator](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.operator(VS.80).aspx) a `GreaterThanEqual`, l'oggetto [proprietà ValueToCompare](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.valuetocompare(VS.80).aspx) "0" e il relativo [ Proprietà tipo](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basecomparevalidator.type.aspx) a `Currency`. Il seguente viene illustrata la sintassi dichiarativa di `UnitPrice` del TemplateField `EditItemTemplate` dopo avere apportate queste modifiche:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample3.aspx)]

Dopo aver apportato queste modifiche, aprire la pagina in un browser. Se si tenta di omettere il nome o immettere un valore del prezzo non valido durante la modifica di un prodotto, viene visualizzato un asterisco accanto la casella di testo. Come illustrato nella figura 8, che include il simbolo di valuta, ad esempio $19,95 un valore del prezzo è considerato non valido. Il controllo CompareValidator `Currency` `Type` consente di separatori di cifre (ad esempio virgole o punti, a seconda delle impostazioni cultura) e un segno iniziale o meno, ma *non* consentire un simbolo di valuta. Questo comportamento potrebbe perplex gli utenti quando esegue il rendering attualmente l'interfaccia per la modifica di `UnitPrice` utilizzando il formato di valuta.

> [!NOTE]
> Tenere presente che nel *gli eventi associati ad inserimento, aggiornamento ed eliminazione* esercitazione viene impostato il BoundField `DataFormatString` proprietà `{0:c}` per formattarlo come valuta. Inoltre, viene impostato il `ApplyFormatInEditMode` proprietà su true, causando GridView di modifica della interfaccia per formattare il `UnitPrice` come valuta. Quando si convertono i BoundField in un TemplateField, Visual Studio annotato queste impostazioni e la casella di testo formattato `Text` proprietà come valuta utilizzando la sintassi di associazione dati `<%# Bind("UnitPrice", "{0:c}") %>`.


[![Viene visualizzato un asterisco accanto a caselle di testo con Input non valido](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image23.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image22.png)

**Figura 8**: un asterisco visualizzato Avanti per caselle di testo con Input non valido ([fare clic per visualizzare l'immagine ingrandita](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image24.png))


Durante la convalida funziona come-è, l'utente deve rimuovere manualmente il simbolo di valuta, quando si modifica un record, che non è accettabile. Per risolvere questo problema, sono disponibili tre opzioni:

1. Configurare il `EditItemTemplate` in modo che il `UnitPrice` valore non è formattato come valuta.
2. Consente all'utente di immettere un simbolo di valuta rimuovendo il CompareValidator e sostituirlo con un RegularExpressionValidator che controlla in modo corretto per un valore di valuta formattato correttamente. In questo caso il problema è che l'espressione regolare per convalidare un valore di valuta non è vera e propria e richiede la scrittura di codice se si desidera incorporare impostazioni cultura.
3. Rimuovere completamente il controllo di convalida e si basano sulla logica di convalida sul lato server in GridView `RowUpdating` gestore dell'evento.

Proviamo l'opzione #1 per questo esercizio. Attualmente il `UnitPrice` sia formattata come una valuta l'espressione di associazione dati per la casella di testo in a causa del `EditItemTemplate`: `<%# Bind("UnitPrice", "{0:c}") %>`. Modificare l'istruzione di operazione di binding in `Bind("UnitPrice", "{0:n2}")`, che formatta il risultato sotto forma di numero con due cifre di precisione. Può eseguire questa operazione direttamente tramite la sintassi dichiarativa oppure facendo clic sul collegamento Modifica DataBindings dal `EditUnitPrice` nella casella di testo il `UnitPrice` del TemplateField `EditItemTemplate` (vedere figure 9 e 10).


[![Fare clic sul collegamento Modifica DataBindings del controllo TextBox](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image26.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image25.png)

**Figura 9**: fare clic sul collegamento Modifica DataBindings del controllo TextBox ([fare clic per visualizzare l'immagine ingrandita](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image27.png))


[![Specificare l'identificatore di formato nell'istruzione Bind](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image29.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image28.png)

**Figura 10**: specificare l'identificatore di formato nel `Bind` istruzione ([fare clic per visualizzare l'immagine ingrandita](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image30.png))


Con questa modifica, il prezzo formattato nell'interfaccia di modifica include una virgola come separatore di gruppo e un punto come separatore decimale, ma esclude il simbolo di valuta.

> [!NOTE]
> Il `UnitPrice` `EditItemTemplate` non include un quale consentendo il postback insorgere e la logica di aggiornamento per iniziare. Tuttavia, il `RowUpdating` copiato dal gestore dell'evento di *esaminando gli eventi associati con l'inserimento, aggiornamento ed eliminazione* esercitazione comprende un controllo a livello di programmazione che assicura che il `UnitPrice` viene fornito. È possibile rimuovere questa logica, lasciare il campo in come-, o aggiungere un controllo RequiredFieldValidator per il `UnitPrice` `EditItemTemplate`.


## <a name="step-4-summarizing-data-entry-problems"></a>Passaggio 4: Riepilogo dei problemi nell'immissione dati

Oltre ai controlli di convalida di cinque, ASP.NET include il [controllo ValidationSummary](https://msdn.microsoft.com/library/f9h59855(VS.80).aspx), che consente di visualizzare il `ErrorMessage` s tali controlli di convalida rilevati dati non validi. Questo riepilogo dati possono essere visualizzati come testo nella pagina web o tramite una finestra di messaggio modale, sul lato client. Consente di migliorare questa esercitazione per includere un messagebox sul lato client, il riepilogo di eventuali problemi di convalida.

A tale scopo, trascinare un controllo ValidationSummary dalla casella degli strumenti nella finestra di progettazione. Il percorso del controllo di convalida non è importante, poiché si intende configurare in modo da visualizzare solo il riepilogo come un messagebox. Dopo aver aggiunto il controllo, impostare il relativo [proprietà ShowSummary](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showsummary(VS.80).aspx) per `False` e il relativo [proprietà ShowMessageBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showmessagebox(VS.80).aspx) a `True`. Con l'aggiunta, eventuali errori di convalida sono riepilogati in una finestra di messaggio sul lato client.


[![Gli errori di convalida sono riepilogati in una finestra di messaggio sul lato Client](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image32.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image31.png)

**Figura 11**: di errori di convalida sono riepilogati in una finestra di messaggio sul lato Client ([fare clic per visualizzare l'immagine ingrandita](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image33.png))


## <a name="step-5-adding-the-validation-controls-to-the-detailsviewsinsertitemtemplate"></a>Passaggio 5: Aggiunta di controlli di convalida al controllo DetailsView.`InsertItemTemplate`

Per questa esercitazione è comunque per aggiungere i controlli di convalida all'interfaccia di inserimento di DetailsView. Il processo di aggiunta di controlli di convalida per i modelli di DetailsView è identico a quello esaminato nel passaggio 3; Pertanto, si sarà anche per l'attività in questo passaggio. Come è stato fatto con il controllo GridView `EditItemTemplate` s, si consiglia di rinominare il `ID` s delle caselle di testo di nondescript `TextBox1` e `TextBox2` a `InsertProductName` e `InsertUnitPrice`.

Aggiungere un controllo RequiredFieldValidator per il `ProductName` `InsertItemTemplate`. Impostare il `ControlToValidate` per il `ID` della casella di testo nel modello, il `Text` proprietà "\*" e il relativo `ErrorMessage` proprietà su "È necessario fornire il nome del prodotto".

Poiché il `UnitPrice` è richiesto per questa pagina quando si aggiunge un nuovo record, aggiungere un controllo RequiredFieldValidator per il `UnitPrice` `InsertItemTemplate`, l'impostazione relativa `ControlToValidate`, `Text`, e `ErrorMessage` proprietà in modo appropriato. Infine, aggiungere un controllo CompareValidator per il `UnitPrice` `InsertItemTemplate` , nonché la configurazione relativa `ControlToValidate`, `Text`, `ErrorMessage`, `Type`, `Operator`, e `ValueToCompare` proprietà esattamente come è stato eseguito con il `UnitPrice`del CompareValidator in GridView `EditItemTemplate`.

Dopo l'aggiunta di questi controlli di convalida, un nuovo prodotto non può essere aggiunti al sistema se non viene specificato il nome o se il prezzo è un numero negativo o formattato in modo non valido.


[![La logica di convalida è stato aggiunto all'interfaccia di inserimento del controllo DetailsView.](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image35.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image34.png)

**Figura 12**: logica di convalida è stato aggiunto all'interfaccia di inserimento di DetailsView ([fare clic per visualizzare l'immagine ingrandita](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image36.png))


## <a name="step-6-partitioning-the-validation-controls-into-validation-groups"></a>Passaggio 6: Partizionamento dei controlli di convalida in gruppi di convalida

La pagina è costituita da due set logicamente diversi controlli di convalida: quelli che corrispondono a GridView della modifica dell'interfaccia e quelli che corrispondono al controllo DetailsView dell'inserimento dell'interfaccia. Per impostazione predefinita, quando si verifica un postback *tutti* vengono controllati i controlli di convalida della pagina. Tuttavia, quando si modifica un record non vogliamo controlli di convalida dell'interfaccia inserimento del controllo DetailsView da convalidare. Figura 13 illustra il problema corrente quando un utente sta modificando un prodotto con i valori validi perfettamente, facendo clic su Update provoca un errore di convalida perché i valori di nome e il prezzo nell'interfaccia di inserimento sono vuoti.


[![L'aggiornamento di un prodotto comporta i controlli di convalida dell'interfaccia inserimento attivati](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image38.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image37.png)

**Figura 13**: l'aggiornamento di un prodotto comporta i controlli di convalida dell'interfaccia inserimento incendio ([fare clic per visualizzare l'immagine ingrandita](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image39.png))


I controlli di convalida in ASP.NET 2.0 possono essere partizionati in gruppi di convalida tramite i relativi `ValidationGroup` proprietà. Per associare un set di controlli di convalida in un gruppo, è sufficiente impostare i relativi `ValidationGroup` proprietà sullo stesso valore. Per questa esercitazione, impostare il `ValidationGroup` le proprietà dei controlli di convalida in GridView TemplateFields a `EditValidationControls` e `ValidationGroup` proprietà TemplateFields del controllo DetailsView per `InsertValidationControls`. Queste modifiche possono essere eseguite direttamente nel markup dichiarativo o tramite la finestra Proprietà quando si utilizza la finestra di progettazione modificare l'interfaccia di modello.

Oltre alla convalida controlli, il pulsante e correlati a pulsanti in ASP.NET 2.0 inoltre includere un `ValidationGroup` proprietà. Validator del gruppo di convalida viene verificato la validità solo quando un postback è causato da un pulsante che ha lo stesso `ValidationGroup` l'impostazione della proprietà. Ad esempio, in ordine per il pulsante Inserisci di DetailsView attivare il `InsertValidationControls` gruppo di convalida, è necessario impostare il CommandField `ValidationGroup` proprietà `InsertValidationControls` (vedere Figura 14). Inoltre, impostare il controllo GridView del CommandField `ValidationGroup` proprietà `EditValidationControls`.


[![Set di DetailsView's proprietà ValidationGroup del CommandField InsertValidationControls](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image41.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image40.png)

**Figura 14**: Set di DetailsView del CommandField `ValidationGroup` proprietà `InsertValidationControls` ([fare clic per visualizzare l'immagine ingrandita](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image42.png))


Dopo aver apportato queste modifiche, il controllo DetailsView GridView TemplateFields e CommandFields dovrebbe essere simile al seguente:

TemplateFields e CommandField del controllo DetailsView.

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample4.aspx)]

Il GridView CommandField e TemplateFields

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample5.aspx)]

A questo punto i controlli di convalida specifica della modifica attivano solo quando viene selezionato il pulsante di aggiornamento del controllo GridView e spegnimento incendi di controlli di convalida specifica insert solo quando si fa clic sul pulsante Inserisci di DetailsView, risolto il problema evidenziato da figura 13. Tuttavia, con questa modifica del controllo ValidationSummary non viene più visualizzato quando si immettono dati non validi. Il controllo ValidationSummary contiene anche un `ValidationGroup` solo Mostra informazioni di riepilogo per i controlli di convalida nel relativo gruppo di convalida e di proprietà. Pertanto, è necessario disporre di due controlli di convalida in questa pagina, uno per il `InsertValidationControls` gruppo di convalida e uno per `EditValidationControls`.

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample6.aspx)]

Con l'aggiunta è stata completata l'esercitazione.

## <a name="summary"></a>Riepilogo

Mentre BoundField può fornire sia un'interfaccia di inserimento e modifica, l'interfaccia non è personalizzabile. In genere, si desidera aggiungere i controlli di convalida per la modifica e l'inserimento di interfaccia per assicurarsi che l'utente immette input obbligatori in un formato valido. A tale scopo è necessario convertire il BoundField TemplateFields e aggiungere i controlli di convalida per i modelli appropriati. In questa esercitazione è stato esteso l'esempio dal *esaminando gli eventi associati con l'inserimento, aggiornamento ed eliminazione* dell'esercitazione, aggiunta di controlli di convalida al controllo DetailsView inserimento di interfaccia e il controllo GridView interfaccia di modifica. Inoltre, è stato illustrato come visualizzare le informazioni di riepilogo di convalida mediante il controllo ValidationSummary e su come partizionare i controlli di convalida della pagina in gruppi distinti di convalida.

Come abbiamo visto in questa esercitazione, TemplateFields consente le interfacce di modificare e l'inserimento di essere ampliate per includere i controlli di convalida. TemplateFields inoltre può essere esteso per includere i controlli Web di input aggiuntivi, abilitare la casella di testo da sostituire con un controllo Web più adatto. Nell'esercitazione successiva vedremo come sostituire il controllo TextBox con controllo DropDownList associato a dati, che è ideale per la modifica di una chiave esterna (ad esempio `CategoryID` o `SupplierID` nel `Products` tabella).

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Lead revisori per questa esercitazione sono stati Liz Shulok e Zack Jones. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Precedente](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md)
[Successivo](customizing-the-data-modification-interface-vb.md)
