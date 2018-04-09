---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/adding-validation-controls-to-the-datalist-s-editing-interface-cs
title: Aggiunta di controlli di convalida a DataList di modifica della interfaccia (c#) | Documenti Microsoft
author: rick-anderson
description: In questa esercitazione si vedrà quanto sia facile aggiungere i controlli di convalida EditItemTemplate del controllo DataList per fornire un int. utente di modifica più infallibile...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: 3ecc21c5-da0e-40ab-abb4-fac1e47398ad
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/adding-validation-controls-to-the-datalist-s-editing-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: 13582989aed60ec7949afcd683472546a91d171c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="adding-validation-controls-to-the-datalists-editing-interface-c"></a>Aggiunta di controlli di convalida all'interfaccia di modifica del controllo DataList (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare App di esempio](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_39_CS.exe) o [Scarica il PDF](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/datatutorial39cs1.pdf)

> In questa esercitazione verranno esaminati quanto sia facile aggiungere i controlli di convalida EditItemTemplate del controllo DataList per fornire un'interfaccia utente di modifica più infallibile.


## <a name="introduction"></a>Introduzione

In DataList modifica finora esercitazioni, la modifica delle interfacce di DataList non includono alcuna convalida dell'input utente attiva anche se l'input utente non valido, ad esempio un nome di prodotto mancanti o i risultati di prezzo negativo di un'eccezione. Nel [esercitazione precedente](handling-bll-and-dal-level-exceptions-cs.md) abbiamo esaminato come aggiungere codice per il controllo DataList s di gestione delle eccezioni `UpdateCommand` gestore eventi per rilevare e normalmente visualizzare informazioni su tutte le eccezioni che sono stati generati. Idealmente, l'interfaccia di modifica include tuttavia i controlli di convalida per impedire un utente di immettere in primo luogo tali dati non validi.

In questa esercitazione si vedrà quanto sia facile aggiungere i controlli di convalida a DataList s `EditItemTemplate` per fornire un'interfaccia utente di modifica più infallibile. In particolare, in questa esercitazione accetta nell'esempio viene creato nell'esercitazione precedente e aggiunge l'interfaccia di modifica per includere la convalida appropriata.

## <a name="step-1-replicating-the-example-fromhandling-bll--and-dal-level-exceptionshandling-bll-and-dal-level-exceptions-csmd"></a>Passaggio 1: La replica di esempio dal[la gestione delle eccezioni BLL e DAL livello](handling-bll-and-dal-level-exceptions-cs.md)

Nel [BLL - gestione e le eccezioni di livello DAL](handling-bll-and-dal-level-exceptions-cs.md) esercitazione è stata creata una pagina elencati i nomi e i prezzi dei prodotti in un controllo DataList due colonne, modificabile. L'obiettivo di questa esercitazione è per aumentare l'interfaccia di modifica s DataList per includere i controlli di convalida. In particolare, la logica di convalida sarà:

- Che è necessario specificare il nome del prodotto s
- Verificare che il valore immesso per il prezzo è un formato di valuta validi
- Assicurarsi che il valore immesso per il prezzo è maggiore o uguale a zero, a partire da un valore negativo `UnitPrice` valore non è valido

Prima di poter esaminare in modo da integrare l'esempio precedente per includere la convalida, è innanzitutto necessario replicare l'esempio dal `ErrorHandling.aspx` nella pagina di `EditDeleteDataList` cartella alla pagina per questa esercitazione, `UIValidation.aspx`. A tale scopo è necessario copiare sia la `ErrorHandling.aspx` pagina dichiarativo s e il relativo codice sorgente. Copiare innanzitutto il markup dichiarativo eseguendo i passaggi seguenti:

1. Aprire il `ErrorHandling.aspx` pagina in Visual Studio
2. Passare alla pagina s dichiarativo (fare clic sul pulsante nella parte inferiore della pagina di origine)
3. Copiare il testo all'interno di `<asp:Content>` e `</asp:Content>` tag (righe 3 e 32), come illustrato nella figura 1.


[![Copiare il testo all'interno del &lt;asp: Content&gt; controllo](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image2.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image1.png)

**Figura 1**: copiare il testo all'interno del `<asp:Content>` controllo ([fare clic per visualizzare l'immagine ingrandita](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image3.png))


1. Aprire il `UIValidation.aspx` pagina
2. Passare al codice dichiarativo pagina s
3. Incollare il testo all'interno di `<asp:Content>` controllo.

Per copiare il codice sorgente, aprire il `ErrorHandling.aspx.vb` pagina e copiare solo il testo *all'interno di* la `EditDeleteDataList_ErrorHandling` classe. Copiare i tre gestori eventi (`Products_EditCommand`, `Products_CancelCommand`, e `Products_UpdateCommand`) insieme al `DisplayExceptionDetails` (metodo), ma non **non** copiare la dichiarazione di classe o `using` istruzioni. Incollare il testo copiato *all'interno di* il `EditDeleteDataList_UIValidation` classe `UIValidation.aspx.vb`.

Dopo avere spostato il contenuto e il codice da `ErrorHandling.aspx` a `UIValidation.aspx`, dedicare alcuni minuti per testare le pagine in un browser. Vedere lo stesso output e che si verifichi la stessa funzionalità in ognuna di queste due pagine (vedere la figura 2).


[![La pagina UIValidation.aspx riproduce la funzionalità in ErrorHandling.aspx](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image5.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image4.png)

**Figura 2**: il `UIValidation.aspx` pagina riproduce la funzionalità in `ErrorHandling.aspx` ([fare clic per visualizzare l'immagine ingrandita](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image6.png))


## <a name="step-2-adding-the-validation-controls-to-the-datalist-s-edititemtemplate"></a>Passaggio 2: Aggiunta di controlli di convalida a EditItemTemplate s DataList

Quando si crea il form di immissione dati, è importante che gli utenti immettere tutti i campi obbligatori e che tutti gli input forniti sono valori validi, formattato correttamente. Per garantire che un input s utente sono validi, ASP.NET fornisce cinque i controlli di convalida incorporate che sono progettati per convalidare il valore di un singolo controllo Web di input:

- [RequiredFieldValidator](https://msdn.microsoft.com/library/5hbw267h(VS.80).aspx) assicura che è stato fornito un valore
- [CompareValidator](https://msdn.microsoft.com/library/db330ayw(VS.80).aspx) convalida un valore rispetto a un altro valore di controllo Web o un valore costante o assicura che il formato del valore s sia valido per un tipo di dati specificato
- [RangeValidator](https://msdn.microsoft.com/library/f70d09xt.aspx) assicura che un valore sia all'interno di un intervallo di valori
- [RegularExpressionValidator](https://msdn.microsoft.com/library/eahwtc9e.aspx) convalida un valore rispetto a un [espressioni regolari](http://en.wikipedia.org/wiki/Regular_expression)
- [CustomValidator](https://msdn.microsoft.com/library/9eee01cx(VS.80).aspx) convalida un valore rispetto a un metodo personalizzato definito dall'utente

Per ulteriori informazioni su questi cinque controlli si riferiscono nuovamente al [aggiunta di controlli di convalida per la modifica e inserimento di interfacce](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) esercitazione o check-out di [sezione relativa ai controlli di convalida](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/validation/default.aspx) del [Esercitazioni delle Guide rapide ASP.NET](https://quickstarts.asp.net).

Per questa esercitazione è necessario utilizzare RequiredFieldValidator per garantire che è stato fornito un valore per il nome del prodotto e un controllo CompareValidator per assicurarsi che il prezzo immesso con un valore maggiore o uguale a 0 e verrà presentato in un formato di valuta validi.

> [!NOTE]
> Mentre ASP.NET 1. x era questi stessi controlli di convalida di cinque, ASP.NET 2.0 ha aggiunto una serie di miglioramenti, principale due script sul lato client da supportare per browser oltre a Internet Explorer e la possibilità di controlli di convalida di partizione in una pagina in gruppi di convalida. Per ulteriori informazioni sulle nuove funzionalità di controllo di convalida in 2.0, fare riferimento a [Sezionando i controlli di convalida in ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx).


Consente di iniziare, aggiungere i controlli di convalida necessari per DataList s s `EditItemTemplate`. Questa attività può essere eseguita tramite la finestra di progettazione facendo clic sul collegamento Modifica modelli dal tag smart s DataList o tramite la sintassi dichiarativa. Consentire il passaggio s tramite il processo utilizzando l'opzione di modifica modelli dalla visualizzazione progettazione. Dopo aver scelto di modificare DataList s `EditItemTemplate`, aggiungere un controllo RequiredFieldValidator trascinandolo dalla casella degli strumenti all'interfaccia di modifica del modello, posizionarlo dopo il `ProductName` casella di testo.


[![Aggiungere un controllo RequiredFieldValidator EditItemTemplate dopo la casella di testo ProductName](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image8.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image7.png)

**Figura 3**: aggiungere un controllo RequiredFieldValidator per il `EditItemTemplate After` il `ProductName` casella di testo ([fare clic per visualizzare l'immagine ingrandita](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image9.png))


Tutti i controlli di convalida di lavoro tramite la convalida dell'input di un singolo controllo Web ASP.NET. Pertanto, è necessario indicare che è stato appena aggiunto RequiredFieldValidator deve convalidare il `ProductName` TextBox; questa operazione viene eseguita impostando il controllo di convalida s [ `ControlToValidate` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.controltovalidate(VS.80).aspx) per il `ID` di il controllo Web appropriato (`ProductName`, in questa istanza). Successivamente, impostare il [ `ErrorMessage` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.errormessage(VS.80).aspx) per specificare il nome del prodotto s e [ `Text` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.text(VS.80).aspx) a \*. Il `Text` valore della proprietà, se fornito, è il testo visualizzato dal controllo di convalida se la convalida non riesce. Il `ErrorMessage` valore della proprietà, è necessario, viene utilizzato il controllo ValidationSummary; se il `Text` valore della proprietà viene omessa, il `ErrorMessage` valore della proprietà viene visualizzato dal controllo di convalida un input non valido.

Dopo l'impostazione di queste tre proprietà di RequiredFieldValidator la schermata dovrebbe essere simile alla figura 4.


[![Impostare le proprietà di testo, un messaggio di errore e un RequiredFieldValidator s ControlToValidate](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image11.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image10.png)

**Figura 4**: impostare s RequiredFieldValidator `ControlToValidate`, `ErrorMessage`, e `Text` delle proprietà ([fare clic per visualizzare l'immagine ingrandita](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image12.png))


Con RequiredFieldValidator aggiunti il `EditItemTemplate`tutti che rimane consiste nell'aggiungere la convalida necessaria per il prezzo del prodotto s casella di testo. Poiché il `UnitPrice` è facoltativa quando si modifica un record, non abbiamo non è necessario aggiungere un controllo RequiredFieldValidator. Tuttavia, è necessario aggiungere un controllo CompareValidator per assicurarsi che il `UnitPrice`, se fornito, viene correttamente formattati come valuta ed è maggiore o uguale a 0.

Aggiungere il controllo CompareValidator nel `EditItemTemplate` e impostare il relativo `ControlToValidate` proprietà `UnitPrice`, le `ErrorMessage` proprietà per il prezzo deve essere maggiore o uguale a zero e non può includere il simbolo di valuta e il relativo `Text` proprietà \*. Per indicare che il `UnitPrice` valore deve essere maggiore o uguale a 0, impostare il s CompareValidator [ `Operator` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.operator(VS.80).aspx) a `GreaterThanEqual`, l'oggetto [ `ValueToCompare` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.valuetocompare(VS.80).aspx) su 0, e il relativo [ `Type` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basecomparevalidator.type.aspx) a `Currency`.

Dopo l'aggiunta di questi due controlli di convalida, DataList s `EditItemTemplate` la sintassi dichiarativa s dovrebbe essere simile al seguente:


[!code-aspx[Main](adding-validation-controls-to-the-datalist-s-editing-interface-cs/samples/sample1.aspx)]

Dopo aver apportato queste modifiche, aprire la pagina in un browser. Se si tenta di omettere il nome o immettere un valore del prezzo non valido durante la modifica di un prodotto, viene visualizzato un asterisco accanto la casella di testo. Come illustrato nella figura 5, che include il simbolo di valuta, ad esempio $19,95 un valore del prezzo è considerato non valido. S CompareValidator `Currency` `Type` consente di separatori di cifre (ad esempio virgole o punti, a seconda delle impostazioni cultura) e un segno iniziale o meno, ma *non* consentire un simbolo di valuta. Questo comportamento potrebbe perplex gli utenti quando esegue il rendering attualmente l'interfaccia per la modifica di `UnitPrice` utilizzando il formato di valuta.


[![Viene visualizzato un asterisco accanto alle caselle di testo con Input non valido](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image14.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image13.png)

**Figura 5**: un asterisco viene visualizzato Avanti per caselle di testo con Input non valido ([fare clic per visualizzare l'immagine ingrandita](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image15.png))


Durante la convalida funziona come-è, l'utente deve rimuovere manualmente il simbolo di valuta, quando si modifica un record, che non è accettabile. Inoltre, se vi sono validi gli input in cui la modifica dell'interfaccia né l'aggiornamento né Annulla pulsanti, quando selezionato, verrà richiamato un postback. In teoria, il pulsante Annulla restituirebbe DataList allo stato di pre-modifica indipendentemente dalla validità degli input utente s. Inoltre, è necessario verificare che i dati della pagina s siano validi prima di aggiornare le informazioni sul prodotto in DataList s `UpdateCommand` gestore eventi, come i controlli di convalida logica sul lato client può essere ignorata da parte degli utenti che usano browser si supporta JavaScript o avranno il supporto è disabilitato.

## <a name="removing-the-currency-symbol-from-the-edititemtemplate-s-unitprice-textbox"></a>Rimuovere il simbolo di valuta dalla casella di testo UnitPrice s EditItemTemplate

Quando si utilizza il s CompareValidator `Currency``Type`, l'input da convalidare non deve includere i simboli di valuta. La presenza di tali simboli determina CompareValidator contrassegnare l'input come non valida. Tuttavia, l'interfaccia di modifica attualmente include un simbolo di valuta nel `UnitPrice` casella di testo, vale a dire l'utente deve rimuovere il simbolo di valuta in modo esplicito prima di salvare le modifiche. Per risolvere questo problema sono disponibili tre opzioni:

1. Configurare il `EditItemTemplate` in modo che il `UnitPrice` valore casella di testo non è formattato come valuta.
2. Consente all'utente di immettere un simbolo di valuta rimuovendo il CompareValidator e sostituirlo con un RegularExpressionValidator che verifica la presenza di un valore di valuta formattato correttamente. La richiesta di verifica qui è che l'espressione regolare per convalidare un valore di valuta non è semplice come quello di CompareValidator e richiede la scrittura di codice se si desidera incorporare impostazioni cultura.
3. Rimuovere completamente il controllo di convalida e si basano sulla logica di convalida sul lato server personalizzato in GridView s `RowUpdating` gestore dell'evento.

Consente di accedere con l'opzione 1 per questa esercitazione s. Attualmente il `UnitPrice` è formattato come un valore di valuta a causa dell'espressione di associazione dati per la casella di testo nel `EditItemTemplate`: `<%# Eval("UnitPrice", "{0:c}") %>`. Modifica il `Eval` istruzione `Eval("UnitPrice", "{0:n2}")`, che formatta il risultato sotto forma di numero con due cifre di precisione. Ciò è possibile direttamente tramite la sintassi dichiarativa o facendo clic sul collegamento Modifica DataBindings dal `UnitPrice` casella di testo in DataList s `EditItemTemplate`.

Con questa modifica, il prezzo formattato nell'interfaccia di modifica include una virgola come separatore di gruppo e un punto come separatore decimale, ma esclude il simbolo di valuta.

> [!NOTE]
> Quando si rimuove il formato valuta dall'interfaccia modificabile, utile inserire il simbolo di valuta il testo all'esterno di casella di testo. Funge da un suggerimento all'utente che non è necessario fornire il simbolo di valuta.


## <a name="fixing-the-cancel-button"></a>Correggere il pulsante Annulla

Per impostazione predefinita, i controlli di convalida Web emit JavaScript per eseguire la convalida sul lato client. Quando si seleziona un pulsante, LinkButton o ImageButton, i controlli di convalida nella pagina vengono controllati sul lato client prima che si verifichi il postback. Nel caso i dati non validi, viene annullato il postback. Per alcuni pulsanti, tuttavia, la validità dei dati potrebbe essere irrilevante; In tal caso, visto il postback annullato a causa di dati non validi è noioso.

Il pulsante Annulla è un esempio. Si supponga che un utente immette i dati non validi, ad esempio omettere il nome del prodotto s e quindi decide t desidera salvare il prodotto dopo che tutti e preme il pulsante Annulla. Attualmente, il pulsante Annulla attiva i controlli di convalida della pagina, i report che il nome del prodotto è manca e impedire il postback. L'utente deve digitare del testo nel `ProductName` casella di testo solo per annullare il processo di modifica.

Per fortuna, il pulsante, LinkButton e ImageButton dispongono di un [ `CausesValidation` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.causesvalidation.aspx) che può indicare se facendo clic sul pulsante deve avviare la logica di convalida (per impostazione predefinita `True`). Impostare i dispositivi del pulsante Annulla `CausesValidation` proprietà `False`.

## <a name="ensuring-the-inputs-are-valid-in-the-updatecommand-event-handler"></a>Verificare gli input sono validi nel gestore eventi UpdateCommand

A causa di script sul lato client generato per i controlli di convalida, se un utente immette un input non valido i controlli di convalida annullare qualsiasi postback iniziati dal pulsante, LinkButton o ImageButton controlli la cui `CausesValidation` sono proprietà `True` (la Per impostazione predefinita). Tuttavia, se un utente visita un browser antiquato oppure a uno cui supporto JavaScript è stato disabilitato, i controlli di convalida sul lato client non verranno eseguita.

Tutti i controlli di convalida ASP.NET ripetere la logica di convalida subito dopo il postback e segnalare la validità complessiva degli input pagina s tramite il [ `Page.IsValid` proprietà](https://msdn.microsoft.com/library/system.web.ui.page.isvalid.aspx). Tuttavia, il flusso di pagina non è interrotta o arrestato in qualsiasi modalità in base al valore di `Page.IsValid`. Gli sviluppatori, ha la responsabilità per garantire che il `Page.IsValid` proprietà ha un valore `True` prima di procedere con il codice che presuppone valido i dati di input.

Se un utente dispone di JavaScript disabilitato, visita la pagina, modifica di un prodotto, immette un valore del prezzo di troppi costose e fa clic sul pulsante di aggiornamento, la convalida lato client verrà ignorata e verrà insorgere un postback. Durante il postback, ASP.NET pagina s `UpdateCommand` gestore eventi viene eseguito e viene generata un'eccezione durante il tentativo di analizzare troppo costoso un `Decimal`. Poiché è disponibile la gestione delle eccezioni, tale eccezione verrà gestita correttamente, ma è possibile impedire che i dati non validi che slittano tramite in primo luogo da solo procedere con il `UpdateCommand` gestore dell'evento se `Page.IsValid` ha un valore di `True`.

Aggiungere il codice seguente all'inizio del `UpdateCommand` gestore dell'evento, immediatamente prima di `Try` blocco:


[!code-csharp[Main](adding-validation-controls-to-the-datalist-s-editing-interface-cs/samples/sample2.cs)]

Con l'aggiunta, il prodotto tenterà di essere aggiornata solo se i dati inviati sono validi. La maggior parte degli utenti ha vinto t essere in grado di eseguire il postback di dati non valido a causa di script lato client controlli di convalida, ma gli utenti che usano browser si supporta JavaScript o che dispongono di JavaScript supportano disabilitato, possono ignorare i controlli sul lato client e inviare dati non validi.

> [!NOTE]
> Il lettore attenti ricorderà che quando l'aggiornamento dei dati in GridView, ci non è necessario verificare in modo esplicito il `Page.IsValid` proprietà la classe code-behind pagina s. In questo modo il controllo GridView consulta il `Page.IsValid` proprietà per noi e solo prosegue con l'aggiornamento solo se viene restituito un valore di `True`.


## <a name="step-3-summarizing-data-entry-problems"></a>Passaggio 3: Riepilogo dei problemi nell'immissione dati

Oltre ai controlli di convalida di cinque, ASP.NET include il [controllo ValidationSummary](https://msdn.microsoft.com/library/f9h59855(VS.80).aspx), che consente di visualizzare il `ErrorMessage` s tali controlli di convalida rilevati dati non validi. Questo riepilogo dati possono essere visualizzati come testo nella pagina web o tramite una finestra di messaggio modale, sul lato client. Consente di migliorare in questa esercitazione per includere il riepilogo di eventuali problemi di convalida sul lato client messagebox s.

A tale scopo, trascinare un controllo ValidationSummary dalla casella degli strumenti nella finestra di progettazione. Il percorso di t ValidationSummary controllo è importante, poiché è re prevede di configurare in modo da visualizzare solo il riepilogo come un messagebox. Dopo aver aggiunto il controllo, impostare il relativo [ `ShowSummary` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showsummary(VS.80).aspx) a `False` e il relativo [ `ShowMessageBox` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showmessagebox(VS.80).aspx) a `True`. Con l'aggiunta, eventuali errori di convalida sono riepilogati in una finestra di messaggio sul lato client (vedere Figura 6).


[![Gli errori di convalida sono riepilogati in Messagebox lato Client](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image17.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image16.png)

**Figura 6**: di errori di convalida sono riepilogati in Messagebox lato Client ([fare clic per visualizzare l'immagine ingrandita](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image18.png))


## <a name="summary"></a>Riepilogo

In questa esercitazione è stato illustrato come ridurre la probabilità di eccezioni utilizzando i controlli di convalida per verificare in modo proattivo che l'input degli utenti validi prima di tentare di utilizzarli nel flusso di lavoro di aggiornamento. ASP.NET fornisce cinque controlli Web di convalida che sono progettati per controllare un determinato Web controllano input s e segnalano la validità dell'input s. In questa esercitazione è possibile usare due di questi cinque controlli RequiredFieldValidator e il controllo CompareValidator per assicurarsi che sia stato specificato il nome del prodotto s e che il prezzo ha un formato di valuta con un valore maggiore o uguale a zero.

Aggiunta di controlli di convalida al s DataList modifica interfaccia è semplice come trascinarli nella `EditItemTemplate` dalla casella degli strumenti e l'impostazione di un numero limitato di proprietà. Per impostazione predefinita, i controlli di convalida generano automaticamente script di convalida lato client. Oltre a fornire la convalida sul lato server durante il postback, archiviando il risultato cumulativo nel `Page.IsValid` proprietà. Per ignorare la convalida lato client quando si seleziona un pulsante, LinkButton o ImageButton, impostare il pulsante s `CausesValidation` proprietà `False`. Inoltre, prima di eseguire qualsiasi attività con i dati inviati durante il postback, verificare che il `Page.IsValid` restituisce proprietà `True`.

Tutti i DataList modifica esercitazioni si ve esaminata finora sono stati molto semplice interfacce modificare una casella di testo per il nome del prodotto s e un altro per il prezzo. L'interfaccia di modifica, tuttavia, può contenere una combinazione di diversi controlli Web, ad esempio controlli DropDownList, calendari, pulsanti di opzione, caselle di controllo e così via. Nell'esercitazione successiva verrà esaminato la creazione di un'interfaccia che utilizza un'ampia gamma di controlli Web.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Lead revisori per questa esercitazione sono stati Dennis Patterson Ken Pespisa e Liz Shulok. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](handling-bll-and-dal-level-exceptions-cs.md)
> [Successivo](customizing-the-datalist-s-editing-interface-cs.md)
