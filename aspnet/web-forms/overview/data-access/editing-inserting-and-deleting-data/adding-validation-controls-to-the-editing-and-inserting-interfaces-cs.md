---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs
title: Aggiunta di controlli di convalida per il processo di modifica e l'inserimento di interfacce (c#) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione verrà illustrato come è facile aggiungere i controlli di convalida a EditItemTemplate e InsertItemTemplate di un controllo Web, dei dati per offrire maggiori...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 2086cb1a-ab78-49ae-9c0b-03891c69776a
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs
msc.type: authoredcontent
ms.openlocfilehash: c7150ca81d77d7d5196ad9e2969c5d7508477267
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37384225"
---
<a name="adding-validation-controls-to-the-editing-and-inserting-interfaces-c"></a>Aggiunta di controlli di convalida per il processo di modifica e l'inserimento di interfacce (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'App di esempio](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_19_CS.exe) o [Scarica il PDF](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/datatutorial19cs1.pdf)

> In questa esercitazione verrà illustrato come è facile aggiungere i controlli di convalida a EditItemTemplate e InsertItemTemplate di un controllo Web, dei dati per fornire un'interfaccia utente più infallibile.


## <a name="introduction"></a>Introduzione

I controlli GridView e DetailsView negli esempi abbiamo esplorato negli ultimi tre esercitazioni hanno tutti stato composto BoundField e CheckBoxFields (i tipi di campi aggiunti automaticamente da Visual Studio quando si associa un controllo GridView o DetailsView a un'origine dati controllare tramite lo smart tag). Quando si modifica una riga in un controllo GridView o DetailsView, questi BoundField che non sono di sola lettura vengono convertiti nelle caselle di testo, da cui gli utenti finali possono modificare i dati esistenti. Analogamente, quando inserire un nuovo record in un controllo DetailsView, questi BoundField la cui proprietà `InsertVisible` è impostata su `true` (predefinito) vengono visualizzati come caselle di testo vuota in cui l'utente può fornire valori di campo del nuovo record. CheckBoxFields, che sono disabilitate nell'interfaccia standard, di sola lettura, allo stesso modo, vengono convertiti in abilitata caselle di controllo di modifica e inserimento di interfacce.

Mentre il valore predefinito di modifica e inserimento di interfacce per i BoundField e CampoCasellaDiControllo può essere utile, l'interfaccia non dispone di alcun tipo di convalida. Se un utente commette un errore data voce - ad esempio l'omissione di `ProductName` campo o l'immissione di un valore valido per `UnitsInStock` (ad esempio -50) verrà generata un'eccezione da entro le profondità dell'architettura dell'applicazione. Anche se questa eccezione può essere gestita correttamente come illustrato nel [esercitazione precedente](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md), in teoria il processo di modifica o l'inserimento dell'interfaccia utente includono i controlli di convalida per impedire un utente di immettere tali dati non validi nel Per prima cosa salvare.

Per fornire una modifica personalizzata o un'interfaccia di inserimento, è necessario sostituire i BoundField o CampoCasellaDiControllo con un TemplateField. TemplateFields, ovvero l'argomento di discussione nel [uso di TemplateFields nel controllo GridView](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) e [uso di TemplateFields nel controllo DetailsView](../custom-formatting/using-templatefields-in-the-detailsview-control-cs.md) esercitazioni, può essere costituito da più i modelli di definizione di separano le interfacce per gli stati di riga diversi. Il TemplateField `ItemTemplate` viene usato per il rendering di campi di sola lettura o righe nei controlli DetailsView o GridView, mentre la `EditItemTemplate` e `InsertItemTemplate` indicano le interfacce da usare per la modifica e inserimento modalità, rispettivamente.

In questa esercitazione verrà illustrato come è facile aggiungere i controlli di convalida per il TemplateField `EditItemTemplate` e `InsertItemTemplate` per fornire un'interfaccia utente più infallibile. In particolare, questa esercitazione saranno necessari l'esempio creato nel [esaminare gli eventi associati con inserimento, aggiornamento ed eliminazione](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md) esercitazione e accresce la modifica e inserimento delle interfacce per includere la convalida appropriata.

## <a name="step-1-replicating-the-example-fromexamining-the-events-associated-with-inserting-updating-and-deletingexamining-the-events-associated-with-inserting-updating-and-deleting-csmd"></a>Passaggio 1: Replica l'esempio da[analisi degli eventi associati a inserimento, aggiornamento ed eliminazione](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md)

Nel [esaminare gli eventi associati con inserimento, aggiornamento ed eliminazione](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md) esercitazione viene creata una pagina che elencati i nomi e i prezzi dei prodotti in un controllo GridView modificabile. Inoltre, la pagina è incluso un controllo DetailsView cui `DefaultMode` è stata impostata su `Insert`, pertanto è sempre il rendering in modalità di inserimento. Da questo controllo DetailsView, l'utente può immettere il nome e il prezzo per un nuovo prodotto, fare clic su Inserisci e aggiungerlo al sistema (vedere la figura 1).


[![Nell'esempio precedente consente agli utenti di aggiungere nuovi prodotti e modificare quelli esistenti](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image2.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image1.png)

**Figura 1**: il precedente esempio consente agli utenti di aggiungere nuovi prodotti e modificare quelli esistenti ([fare clic per visualizzare l'immagine con dimensioni normali](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image3.png))


L'obiettivo di questa esercitazione è per aumentare il controllo DetailsView e GridView per fornire i controlli di convalida. In particolare, la logica di convalida sarà:

- Richiedere che il nome deve essere fornito durante l'inserimento o modifica di un prodotto
- Richiedi che il prezzo deve essere fornito quando si inserisce un record. Quando si modifica un record, si sarà comunque necessario un prezzo, ma userà la logica a livello di codice del controllo GridView `RowUpdating` gestore dell'evento sono già presenti nell'esercitazione precedente
- Assicurarsi che il valore immesso per il prezzo è un formato di valuta validi

Prima di poter esaminare in modo da integrare l'esempio precedente per includere la convalida, prima è necessario eseguire la replica nell'esempio il `DataModificationEvents.aspx` pagina per pagina per questa esercitazione, `UIValidation.aspx`. A tale scopo è necessario copiare entrambi i `DataModificationEvents.aspx` markup dichiarativo della pagina e il relativo codice sorgente. Copiare prima di tutto il markup dichiarativo attenendosi alla procedura seguente:

1. Aprire il `DataModificationEvents.aspx` pagina in Visual Studio
2. Vai al markup della pagina dichiarativo (fare clic sul pulsante di origine nella parte inferiore della pagina)
3. Copiare il testo all'interno di `<asp:Content>` e `</asp:Content>` tag (righe tra 3 e 44), come illustrato nella figura 2.


[![Copiare il testo interno di &lt;asp: Content&gt; controllo](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image5.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image4.png)

**Figura 2**: copiare il testo all'interno del `<asp:Content>` controllo ([fare clic per visualizzare l'immagine con dimensioni normali](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image6.png))


1. Aprire il `UIValidation.aspx` pagina
2. Vai al markup della pagina dichiarativo
3. Incollare il testo all'interno di `<asp:Content>` controllo.

Per copiare il codice sorgente, aprire il `DataModificationEvents.aspx.cs` pagina e copiare semplicemente il testo *entro* il `EditInsertDelete_DataModificationEvents` classe. Copiare i tre gestori eventi (`Page_Load`, `GridView1_RowUpdating`, e `ObjectDataSource1_Inserting`), ma esegue **non** copiare la dichiarazione di classe o `using` istruzioni. Incollare il testo copiato *all'interno* le `EditInsertDelete_UIValidation` classe `UIValidation.aspx.cs`.

Dopo lo spostamento sul contenuto e codice dal `DataModificationEvents.aspx` a `UIValidation.aspx`, si consiglia di testare lo stato di avanzamento in un browser. Dovrebbe essere lo stesso output e si verifichi la stessa funzionalità in ognuna di queste due pagine (vedere la figura 1 per una cattura di schermata della `DataModificationEvents.aspx` in azione).

## <a name="step-2-converting-the-boundfields-into-templatefields"></a>Passaggio 2: Convertire i BoundField in TemplateFields

Per aggiungere i controlli di convalida alle interfacce di modifica e inserimento, devono essere convertiti in TemplateFields i BoundField utilizzato dai controlli GridView e DetailsView. A tale scopo, seleziona i collegamenti modifica colonne e modificare i campi in GridView e DetailsView smart tag, rispettivamente. Selezionare ognuna dei BoundField e fare clic sul collegamento "Converti il campo in un TemplateField".


[![Convertire i BoundField di DetailsView e del controllo GridView in TemplateFields](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image8.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image7.png)

**Figura 3**: convertirli singolarmente il DetailsView e del controllo GridView BoundField in TemplateFields ([fare clic per visualizzare l'immagine con dimensioni normali](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image9.png))


Conversione di un BoundField in un TemplateField tramite la finestra di dialogo campi genera un TemplateField che presenta le stesse interfacce di sola lettura, modifica e inserimento i BoundField stesso. Il markup seguente illustra la sintassi dichiarativa per il `ProductName` campo nel controllo DetailsView dopo che è stato convertito in un TemplateField:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/samples/sample1.aspx)]

Si noti che questo TemplateField disponeva di tre modelli creati automaticamente `ItemTemplate`, `EditItemTemplate`, e `InsertItemTemplate`. Il `ItemTemplate` consente di visualizzare il valore di un campo di dati single (`ProductName`) usando un controllo etichetta Web, mentre le `EditItemTemplate` e `InsertItemTemplate` presentare il valore del campo dati in un controllo TextBox Web che associa il campo dati con la casella di testo `Text` proprietà con associazione dati bidirezionale. Poiché si sta usando solo DetailsView in questa pagina per l'inserimento, è possibile rimuovere il `ItemTemplate` e `EditItemTemplate` da due TemplateFields, anche se non è possibile lasciarli.

Poiché il controllo GridView non supporta predefiniti inserimento di funzionalità del controllo DetailsView, conversione di GridView `ProductName` campo in un TemplateField comporta solo un' `ItemTemplate` e `EditItemTemplate`:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/samples/sample2.aspx)]

Facendo clic su "Converti il campo in un TemplateField", Visual Studio ha creato un TemplateField cui modelli imitare l'interfaccia utente dei BoundField convertito. È possibile verificarlo visitando questa pagina tramite un browser. Sono disponibili che l'aspetto e il comportamento di TemplateFields sia identico all'esperienza quando sono stati usati invece BoundField.

> [!NOTE]
> È possibile personalizzare le interfacce di modifica di modelli in base alle esigenze. Ad esempio, potremmo si vuole che la casella di testo `UnitPrice` TemplateFields sottoposto a rendering come una casella di testo più piccolo rispetto al `ProductName` nella casella di testo. A tale scopo è possibile impostare la casella di testo `Columns` proprietà su un valore appropriato oppure fornire una larghezza assoluta tramite il `Width` proprietà. Nella prossima esercitazione vedremo come personalizzare completamente l'interfaccia di modifica, sostituendo la casella di testo con una voce di dati alternativi controllo Web.


## <a name="step-3-adding-the-validation-controls-to-the-gridviewsedititemtemplate-s"></a>Passaggio 3: Aggiunta di controlli di convalida per il controllo GridView`EditItemTemplate` s

Durante la creazione di form di immissione dati, è importante che gli utenti immettono tutti i campi obbligatori e che tutti gli input specificati sono valori validi, formattata correttamente. Per garantire che gli input dell'utente sono validi, ASP.NET fornisce cinque controlli di convalida incorporate che sono progettati per essere usato per convalidare il valore di un singolo controllo di input:

- [RequiredFieldValidator](https://msdn.microsoft.com/library/5hbw267h(VS.80).aspx) assicura che è stato fornito un valore
- [CompareValidator](https://msdn.microsoft.com/library/db330ayw(VS.80).aspx) convalida un valore rispetto a un altro valore di controllo Web o un valore costante o assicura che il formato del valore è valido per un tipo di dati specificato
- [RangeValidator](https://msdn.microsoft.com/library/f70d09xt.aspx) garantisce che un valore sia entro un intervallo di valori
- [RegularExpressionValidator](https://msdn.microsoft.com/library/eahwtc9e.aspx) convalida di un valore rispetto a un [espressioni regolari](http://en.wikipedia.org/wiki/Regular_expression)
- [CustomValidator](https://msdn.microsoft.com/library/9eee01cx(VS.80).aspx) convalida un valore rispetto a un metodo personalizzato definito dall'utente

Per altre informazioni su questi cinque controlli, consultare il [sezione relativa ai controlli di convalida](https://quickstarts.asp.net/quickstartv20/aspnet/doc/ctrlref/validation/default.aspx) delle [esercitazioni della Guida rapida ASP.NET](https://asp.net/QuickStart/aspnet/).

Per questa esercitazione è necessario usare un RequiredFieldValidator di GridView e DetailsView `ProductName` TemplateFields e un RequiredFieldValidator in DetailsView `UnitPrice` TemplateField. Inoltre, è necessario aggiungere un controllo CompareValidator per entrambi i controlli `UnitPrice` TemplateFields che assicura che il prezzo immesso ha un valore maggiore o uguale a 0 e verrà visualizzato in un formato di valuta validi.

> [!NOTE]
> Mentre ASP.NET 1.x ha questi stessi controlli di cinque convalida, ASP.NET 2.0 ha aggiunto una serie di miglioramenti, principale due script lato client il supporto per i browser diversi da Internet Explorer e la possibilità di controlli di convalida di partizione in una pagina in gruppi di convalida. Per altre informazioni sulle nuove funzionalità di controllo di convalida in 2.0, consultare [Sezionando i controlli di convalida in ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx).


Iniziamo aggiungendo i controlli di convalida necessari per il `EditItemTemplate` s in TemplateFields di GridView. A tale scopo, fare clic sul collegamento di modifica modelli dallo smart tag di GridView per visualizzare l'interfaccia di modifica del modello. A questo punto, è possibile selezionare il modello da modificare nell'elenco a discesa. Poiché si vogliono migliorare l'interfaccia di modifica, è necessario aggiungere i controlli di convalida per il `ProductName` e `UnitPrice`del `EditItemTemplate` s.


[![È necessario estendere il ProductName e EditItemTemplates del prezzo unitario](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image11.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image10.png)

**Figura 4**: È necessario estendere la `ProductName` e `UnitPrice`del `EditItemTemplate` s ([fare clic per visualizzare l'immagine con dimensioni normali](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image12.png))


Nel `ProductName` `EditItemTemplate`, aggiungere un controllo RequiredFieldValidator trascinandolo dalla casella degli strumenti all'interfaccia di modifica del modello, l'inserimento dopo la casella di testo.


[![Aggiungere un RequiredFieldValidator ProductName EditItemTemplate](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image14.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image13.png)

**Figura 5**: aggiungere un RequiredFieldValidator per il `ProductName` `EditItemTemplate` ([fare clic per visualizzare l'immagine con dimensioni normali](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image15.png))


Tutti i controlli di convalida di lavoro per la convalida dell'input di un singolo controllo Web ASP.NET. Di conseguenza, dobbiamo indicare che RequiredFieldValidator appena aggiunto deve convalidare la casella di testo il `EditItemTemplate`; questa operazione viene eseguita impostando il controllo di convalida [ControlToValidate proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.controltovalidate(VS.80).aspx) per il `ID` del controllo Web appropriato. La casella di testo ha attualmente il piuttosto nondescript `ID` di `TextBox1`, ma è possibile modificarlo specificando un nome più appropriato. Fare clic sulla casella di testo nel modello e quindi, dalla finestra delle proprietà, modificare il `ID` dal `TextBox1` a `EditProductName`.


[![Modificare l'ID della casella di testo a EditProductName](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image17.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image16.png)

**Figura 6**: modificare la casella di testo `ID` al `EditProductName` ([fare clic per visualizzare l'immagine con dimensioni normali](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image18.png))


Successivamente, impostare il controllo RequiredFieldValidator `ControlToValidate` proprietà `EditProductName`. Infine, impostare il [proprietà ErrorMessage](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.errormessage(VS.80).aspx) "È necessario fornire il nome del prodotto" e il [proprietà Text](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.text(VS.80).aspx) a "\*". Il `Text` valore della proprietà, se fornito, è il testo visualizzato dal controllo di convalida se la convalida non riesce. Il `ErrorMessage` valore della proprietà, è necessario, viene utilizzata dal controllo di controllo ValidationSummary; se il `Text` valore della proprietà viene omessa, il `ErrorMessage` valore della proprietà è anche il testo visualizzato dal controllo di convalida nell'input non valido.

Dopo aver impostato queste tre proprietà di RequiredFieldValidator, la schermata dovrebbe essere simile alla figura 7.


[![Impostare il controllo RequiredFieldValidator ControlToValidate ErrorMessage e proprietà di testo](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image20.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image19.png)

**Figura 7**: impostare il controllo RequiredFieldValidator `ControlToValidate`, `ErrorMessage`, e `Text` delle proprietà ([fare clic per visualizzare l'immagine con dimensioni normali](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image21.png))


Con RequiredFieldValidator aggiunto per il `ProductName` `EditItemTemplate`, resta per aggiungere la convalida necessaria per il `UnitPrice` `EditItemTemplate`. Poiché Microsoft ha deciso che, per questa pagina, il `UnitPrice` facoltativo quando si modifica un record, non è necessario aggiungere un controllo RequiredFieldValidator. , Tuttavia, dobbiamo aggiungere un controllo CompareValidator per assicurarsi che il `UnitPrice`, se fornito, viene correttamente formattato come valuta ed è maggiore o uguale a 0.

Prima di aggiungere il controllo CompareValidator per i `UnitPrice` `EditItemTemplate`, modifichiamo prima di tutto l'ID del controllo TextBox Web dal `TextBox2` a `EditUnitPrice`. Dopo aver apportato questa modifica, aggiungere il controllo CompareValidator, impostazione relativi `ControlToValidate` proprietà `EditUnitPrice`, la relativa `ErrorMessage` proprietà su "il prezzo deve essere maggiore o uguale a zero e non può includere il simbolo di valuta," e il relativo `Text` proprietà "\*".

Per indicare che il `UnitPrice` valore deve essere maggiore o uguale a 0, impostare il controllo CompareValidator [proprietà Operator](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.operator(VS.80).aspx) a `GreaterThanEqual`, la relativa [ValueToCompare proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.valuetocompare(VS.80).aspx) su "0" e il [ Proprietà di tipo](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basecomparevalidator.type.aspx) a `Currency`. La seguente sintassi dichiarativa per il `UnitPrice` del TemplateField `EditItemTemplate` dopo aver apportate queste modifiche:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/samples/sample3.aspx)]

Dopo aver apportato queste modifiche, aprire la pagina in un browser. Se si tenta di omettere il nome o immettere un valore del prezzo non valido durante la modifica di un prodotto, viene visualizzato un asterisco accanto alla casella di testo. Come illustrato nella figura 8, un valore di prezzo che include il simbolo di valuta, ad esempio 19,95 dollari con acquisto viene considerato non valido. Il controllo CompareValidator `Currency` `Type` consente di separatori di cifre (ad esempio virgole o punti, a seconda delle impostazioni cultura) e un segno iniziale più o meno, a differenza *non* consentono un simbolo di valuta. Questo comportamento potrebbe perplex agli utenti come l'interfaccia di modifica attualmente viene eseguito il rendering di `UnitPrice` utilizzando il formato di valuta.

> [!NOTE]
> Si tenga presente che nel *associati a eventi con inserimento, aggiornamento ed eliminazione in corso* esercitazione impostiamo i BoundField `DataFormatString` proprietà `{0:c}` per formattarlo come una valuta. Inoltre, impostiamo il `ApplyFormatInEditMode` causando GridView interfaccia per la formattazione di modifica della proprietà su true, il `UnitPrice` come valuta. Quando si convertono i BoundField in un TemplateField, Visual Studio annotato queste impostazioni e la casella di testo formattato `Text` proprietà come valuta utilizzando la sintassi di associazione dati `<%# Bind("UnitPrice", "{0:c}") %>`.


[![Viene visualizzato un asterisco accanto alle caselle di testo con Input non valido](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image23.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image22.png)

**Figura 8**: un asterisco viene visualizzata accanto alla caselle di testo con Input non valido ([fare clic per visualizzare l'immagine con dimensioni normali](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image24.png))


Durante il funzionamento della convalida come-è, l'utente deve rimuovere manualmente il simbolo di valuta, quando si modifica un record che non è accettabile. Per risolvere questo problema, sono disponibili tre opzioni:

1. Configurare il `EditItemTemplate` in modo che il `UnitPrice` valore non è formattato come una valuta.
2. Consente all'utente di immettere un simbolo di valuta, rimozione controllo CompareValidator e sostituendolo con un RegularExpressionValidator che controlla in modo corretto per un valore di valuta formattato correttamente. In questo caso il problema è che l'espressione regolare per convalidare un valore di valuta non sia vera e propria e richiede la scrittura di codice se si vuole incorporare impostazioni cultura.
3. Rimuovere completamente il controllo di convalida e si basano sulla logica di convalida sul lato server del controllo GridView `RowUpdating` gestore dell'evento.

Torniamo con l'opzione #1 per questo esercizio. Attualmente le `UnitPrice` viene formattato come una valuta a causa dell'espressione di associazione dati per la casella di testo nel `EditItemTemplate`: `<%# Bind("UnitPrice", "{0:c}") %>`. Modificare l'istruzione di associazione in `Bind("UnitPrice", "{0:n2}")`, che formatta il risultato sotto forma di numero con due cifre di precisione. Ciò può essere eseguita direttamente tramite la sintassi dichiarativa o facendo clic sul collegamento Modifica DataBindings dal `EditUnitPrice` nella casella di testo il `UnitPrice` del TemplateField `EditItemTemplate` (vedere figure 9 e 10).


[![Fare clic sul collegamento Modifica DataBindings di TextBox](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image26.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image25.png)

**Figura 9**: fare clic sul collegamento Modifica DataBindings della casella di testo ([fare clic per visualizzare l'immagine con dimensioni normali](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image27.png))


[![Specificare l'identificatore di formato nell'istruzione binding](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image29.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image28.png)

**Figura 10**: specificare l'identificatore di formato nel `Bind` istruzione ([fare clic per visualizzare l'immagine con dimensioni normali](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image30.png))


Con questa modifica, il prezzo formattato nell'interfaccia di modifica sono incluse le virgole come separatore di gruppo e un punto come separatore decimale, ma lascia disattivare il simbolo di valuta.

> [!NOTE]
> Il `UnitPrice` `EditItemTemplate` non include un RequiredFieldValidator, consentendo l'insorgere del postback e la logica di aggiornamento deve avviare. Tuttavia, il `RowUpdating` gestore di eventi copiati dalla *esaminando gli eventi associati con inserimento, aggiornamento ed eliminazione* esercitazione comprende un controllo a livello di programmazione che assicura che il `UnitPrice` viene fornito. È possibile rimuovere questa logica, lasciare l'impostazione in-viene, o aggiungere un RequiredFieldValidator per il `UnitPrice` `EditItemTemplate`.


## <a name="step-4-summarizing-data-entry-problems"></a>Passaggio 4: Riepilogo dei problemi nell'immissione dati

Oltre ai controlli di cinque convalida, ASP.NET include la [controllo di controllo ValidationSummary](https://msdn.microsoft.com/library/f9h59855(VS.80).aspx), che consente di visualizzare il `ErrorMessage` s tali controlli di convalida che ha rilevato dati non validi. Questi dati di riepilogo possono essere visualizzati come testo della pagina web o tramite una finestra di messaggio modale, lato client. È possibile migliorare in questa esercitazione per includere un messagebox sul lato client, il riepilogo di eventuali problemi di convalida.

A tale scopo, trascinare un controllo di controllo ValidationSummary dalla casella degli strumenti nella finestra di progettazione. La posizione del controllo di convalida non è importante, poiché dobbiamo configurare in modo da essere visualizzato solo il riepilogo come un oggetto messagebox. Dopo aver aggiunto il controllo, impostare relativi [proprietà ShowSummary](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showsummary(VS.80).aspx) a `false` e la relativa [proprietà ShowMessageBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showmessagebox(VS.80).aspx) a `true`. Grazie a questa aggiunta, eventuali errori di convalida sono riepilogati in messagebox lato client.


[![Gli errori di convalida sono riepilogati in Messagebox lato Client](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image32.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image31.png)

**Figura 11**: di errori di convalida sono riepilogati in Messagebox lato Client ([fare clic per visualizzare l'immagine con dimensioni normali](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image33.png))


## <a name="step-5-adding-the-validation-controls-to-the-detailsviewsinsertitemtemplate"></a>Passaggio 5: Aggiunta di controlli di convalida a DetailsView`InsertItemTemplate`

Per questa esercitazione resta che aggiungere i controlli di convalida all'interfaccia di inserimento di DetailsView. Il processo di aggiunta di controlli di convalida per i modelli di DetailsView è identico a quello esaminato nel passaggio 3. Pertanto, si sarà anche per l'attività in questo passaggio. Come è stato fatto con il controllo GridView `EditItemTemplate` s, ti invitiamo a rinominare le `ID` s delle caselle di testo dal nondescript `TextBox1` e `TextBox2` al `InsertProductName` e `InsertUnitPrice`.

Aggiungere un RequiredFieldValidator per il `ProductName` `InsertItemTemplate`. Impostare il `ControlToValidate` per il `ID` della casella di testo nel modello, relativo `Text` proprietà su "\*" e il relativo `ErrorMessage` proprietà su "È necessario fornire il nome del prodotto".

Poiché il `UnitPrice` è obbligatorio per questa pagina quando si aggiunge un nuovo record, aggiungere un RequiredFieldValidator per il `UnitPrice` `InsertItemTemplate`, l'impostazione relativa `ControlToValidate`, `Text`, e `ErrorMessage` proprietà in modo appropriato. Infine, aggiungere un controllo CompareValidator per i `UnitPrice` `InsertItemTemplate` , nonché la configurazione relativa `ControlToValidate`, `Text`, `ErrorMessage`, `Type`, `Operator`, e `ValueToCompare` proprietà come abbiamo fatto con il `UnitPrice`del controllo CompareValidator del controllo GridView `EditItemTemplate`.

Dopo l'aggiunta di questi controlli di convalida, un nuovo prodotto non può essere aggiunti al sistema se il relativo nome viene omesso o se essa il prezzo è un numero negativo o formattato in modo illegale.


[![La logica di convalida è stato aggiunto all'interfaccia di inserimento di DetailsView](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image35.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image34.png)

**Figura 12**: per la logica di convalida è stato aggiunto all'interfaccia di inserimento di DetailsView ([fare clic per visualizzare l'immagine con dimensioni normali](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image36.png))


## <a name="step-6-partitioning-the-validation-controls-into-validation-groups"></a>Passaggio 6: Partizionamento i controlli di convalida in gruppi di convalida

La pagina è costituito da due set logicamente diversi controlli di convalida: quelli che corrispondono a GridView di modifica dell'interfaccia e quelli che corrispondono a DetailsView dell'inserimento di interfaccia. Per impostazione predefinita, quando si verifica un postback *tutti* vengono controllati i controlli di convalida della pagina. Tuttavia, quando si modifica un record non vogliamo i controlli di convalida dell'interfaccia di DetailsView inserimento da convalidare. Figura 13 illustra il dilemma corrente quando un utente sta modificando un prodotto con i valori perfettamente legali, facendo clic su Update provoca un errore di convalida perché i valori nome e il prezzo nell'interfaccia di inserimento sono vuoti.


[![L'aggiornamento di un prodotto fa in modo che i controlli di convalida dell'interfaccia inserimento da generare](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image38.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image37.png)

**Figura 13**: l'aggiornamento di un prodotto fa in modo che i controlli di convalida dell'interfaccia inserimento da generare ([fare clic per visualizzare l'immagine con dimensioni normali](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image39.png))


I controlli di convalida in ASP.NET 2.0 possono essere partizionati in gruppi di convalida tramite i `ValidationGroup` proprietà. Per associare un set di controlli di convalida in un gruppo, è sufficiente impostare i `ValidationGroup` proprietà sullo stesso valore. Per questa esercitazione, impostare il `ValidationGroup` delle proprietà dei controlli di convalida in TemplateFields del controllo GridView a `EditValidationControls` e il `ValidationGroup` delle proprietà di TemplateFields DetailsView per `InsertValidationControls`. Queste modifiche possono essere eseguite direttamente nel markup dichiarativo o tramite la finestra Proprietà quando si usa la finestra di progettazione modificare l'interfaccia del modello.

Oltre alla convalida i controlli, il pulsante e controlli correlati a pulsanti in ASP.NET 2.0 includono anche un `ValidationGroup` proprietà. Validator di un gruppo di convalida viene verificato la validità solo quando viene provocato un postback da un pulsante che ha gli stessi `ValidationGroup` l'impostazione della proprietà. Ad esempio, in ordine per il pulsante Inserisci DetailsView attivare i `InsertValidationControls` gruppo di convalida è necessario impostare il CommandField `ValidationGroup` proprietà `InsertValidationControls` (vedere la figura 14). Inoltre, impostare il controllo GridView di CommandField `ValidationGroup` proprietà `EditValidationControls`.


[![Impostandoli DetailsView proprietà ValidationGroup del CommandField InsertValidationControls](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image41.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image40.png)

**Figura 14**: impostare DetailsView di CommandField `ValidationGroup` proprietà `InsertValidationControls` ([fare clic per visualizzare l'immagine con dimensioni normali](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image42.png))


Dopo tali modifiche, il controllo DetailsView di GridView TemplateFields e CommandFields dovrebbe essere simile al seguente:

TemplateFields e CommandField DetailsView

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/samples/sample4.aspx)]

Il controllo GridView CommandField e TemplateFields

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/samples/sample5.aspx)]

I controlli di convalida specifica della modifica a questo punto vengono attivati solo quando viene selezionato il pulsante di aggiornamento del controllo GridView e il fire controlli di convalida specifica insert solo quando si fa clic sul pulsante Inserisci DetailsView, la risoluzione del problema evidenziato da figura 13. Tuttavia, con questa modifica il controllo di controllo ValidationSummary non visualizza più quando si immettono dati non validi. Il controllo di controllo ValidationSummary contiene anche un `ValidationGroup` proprietà e solo Mostra le informazioni di riepilogo per i controlli di convalida nel relativo gruppo di convalida. Pertanto, è necessario disporre di due controlli di convalida in questa pagina, uno per il `InsertValidationControls` gruppo di convalida e uno per `EditValidationControls`.

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/samples/sample6.aspx)]

Grazie a questa aggiunta seguire l'esercitazione è stata completata.

## <a name="summary"></a>Riepilogo

Mentre BoundField può fornire entrambi un'interfaccia di inserimento e modifica, l'interfaccia non è personalizzabile. In genere, si vuole aggiungere i controlli di convalida per la modifica e l'inserimento dell'interfaccia per assicurarsi che l'utente inserisca gli input necessari in un formato valido. A tale scopo è necessario convertire i BoundField in TemplateFields e aggiungere i controlli di convalida di uno o più modelli appropriato. In questa esercitazione è stato esteso l'esempio dal *esaminare gli eventi associati con inserimento, aggiornamento ed eliminazione* esercitazione, l'aggiunta di controlli di convalida per entrambi DetailsView dell'inserimento di interfaccia e del controllo GridView interfaccia per la modifica. Inoltre, è stato illustrato come visualizzare le informazioni di riepilogo di convalida mediante il controllo di controllo ValidationSummary e come partizionare i controlli di convalida nella pagina in gruppi distinti di convalida.

Come abbiamo visto in questa esercitazione, TemplateFields consentono le interfacce di modifica e inserimento essere ampliate per includere i controlli di convalida. TemplateFields può anche essere esteso per includere controlli Web di input aggiuntivi, abilitare la casella di testo da sostituire con un controllo Web più adatto. Nell'esercitazione successiva si vedrà come sostituire il controllo casella di testo con un controllo DropDownList con associazione a dati, che è ideale quando si modifica una chiave esterna (ad esempio `CategoryID` oppure `SupplierID` nel `Products` tabella).

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. I revisori per questa esercitazione sono Liz Shulok e Zack Jones. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md)
> [Successivo](customizing-the-data-modification-interface-cs.md)
