---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/adding-validation-controls-to-the-datalist-s-editing-interface-cs
title: Aggiunta di controlli di convalida per il controllo DataList di modifica dell'interfaccia (c#) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione verrà illustrato come è facile aggiungere i controlli di convalida al EditItemTemplate di DataList per fornire un int. utente di modifica più infallibile...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: 3ecc21c5-da0e-40ab-abb4-fac1e47398ad
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/adding-validation-controls-to-the-datalist-s-editing-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: 23b7a0a4fd5786d5bbc2905022a76dfc048d44bf
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389955"
---
<a name="adding-validation-controls-to-the-datalists-editing-interface-c"></a>Aggiunta di controlli di convalida all'interfaccia di modifica di DataList (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'App di esempio](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_39_CS.exe) o [Scarica il PDF](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/datatutorial39cs1.pdf)

> In questa esercitazione verrà illustrato come è facile aggiungere i controlli di convalida al EditItemTemplate di DataList per fornire un'interfaccia utente di modifica più infallibile.


## <a name="introduction"></a>Introduzione

In DataList modifica esercitazioni fino ad ora, le interfacce di modifica di DataList non includono una convalida dell'input utente attiva anche se, ad esempio un nome di prodotto mancanti o i risultati di prezzo negativo di un'eccezione di input utente non è valido. Nel [esercitazione precedente](handling-bll-and-dal-level-exceptions-cs.md) abbiamo esaminato come aggiungere codice per il controllo DataList s gestione delle eccezioni `UpdateCommand` gestore eventi per rilevare e normalmente visualizzare informazioni su tutte le eccezioni che sono stati generati. In teoria, tuttavia, l'interfaccia di modifica includerà i controlli di convalida per impedire un utente di immettere tali dati non validi in primo luogo.

In questa esercitazione verrà illustrato come è facile aggiungere i controlli di convalida per il controllo DataList s `EditItemTemplate` per fornire un'interfaccia utente di modifica più infallibile. In particolare, questa esercitazione richiede l'esempio creato nell'esercitazione precedente e aumenta l'interfaccia di modifica per includere la convalida appropriata.

## <a name="step-1-replicating-the-example-fromhandling-bll--and-dal-level-exceptionshandling-bll-and-dal-level-exceptions-csmd"></a>Passaggio 1: Replica l'esempio da[la gestione delle eccezioni a livello BLL e DAL](handling-bll-and-dal-level-exceptions-cs.md)

Nel [BLL - la gestione e le eccezioni a livello DAL](handling-bll-and-dal-level-exceptions-cs.md) esercitazione viene creata una pagina che elencati i nomi e i prezzi dei prodotti in un controllo DataList di due colonne, modificabile. L'obiettivo per questa esercitazione è aumentare l'interfaccia di modifica s DataList per includere i controlli di convalida. In particolare, la logica di convalida sarà:

- Richiedere che il nome del prodotto s essere fornito
- Assicurarsi che il valore immesso per il prezzo è un formato di valuta validi
- Assicurarsi che il valore immesso per il prezzo è maggiore o uguale a zero, poiché un valore negativo `UnitPrice` valore non è valido

Prima di poter esaminare in modo da integrare l'esempio precedente per includere la convalida, prima è necessario eseguire la replica nell'esempio il `ErrorHandling.aspx` nella pagina la `EditDeleteDataList` cartella alla pagina per questa esercitazione, `UIValidation.aspx`. A tale scopo è necessario copiare entrambi i `ErrorHandling.aspx` pagina markup dichiarativo s e il relativo codice sorgente. Copiare prima di tutto il markup dichiarativo attenendosi alla procedura seguente:

1. Aprire il `ErrorHandling.aspx` pagina in Visual Studio
2. Passare alla pagina s dichiarativo (fare clic sul pulsante di origine nella parte inferiore della pagina)
3. Copiare il testo all'interno di `<asp:Content>` e `</asp:Content>` tag (righe tra 3 e 32), come illustrato nella figura 1.


[![Copiare il testo interno di &lt;asp: Content&gt; controllo](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image2.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image1.png)

**Figura 1**: copiare il testo all'interno del `<asp:Content>` controllo ([fare clic per visualizzare l'immagine con dimensioni normali](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image3.png))


1. Aprire il `UIValidation.aspx` pagina
2. Passare al markup dichiarativo s pagina
3. Incollare il testo all'interno di `<asp:Content>` controllo.

Per copiare il codice sorgente, aprire il `ErrorHandling.aspx.vb` pagina e copiare semplicemente il testo *entro* il `EditDeleteDataList_ErrorHandling` classe. Copiare i tre gestori eventi (`Products_EditCommand`, `Products_CancelCommand`, e `Products_UpdateCommand`) con il `DisplayExceptionDetails` metodo, ma si **non** copiare la dichiarazione di classe o `using` istruzioni. Incollare il testo copiato *all'interno* le `EditDeleteDataList_UIValidation` classe `UIValidation.aspx.vb`.

Dopo lo spostamento sul contenuto e codice dal `ErrorHandling.aspx` a `UIValidation.aspx`, si consiglia di testare le pagine in un browser. Si dovrebbe vedere lo stesso output e provare la stessa funzionalità in ognuna di queste due pagine (vedere la figura 2).


[![La pagina UIValidation.aspx riproduce la funzionalità in ErrorHandling.aspx](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image5.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image4.png)

**Figura 2**: il `UIValidation.aspx` pagina riproduce la funzionalità in `ErrorHandling.aspx` ([fare clic per visualizzare l'immagine con dimensioni normali](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image6.png))


## <a name="step-2-adding-the-validation-controls-to-the-datalist-s-edititemtemplate"></a>Passaggio 2: Aggiunta di controlli di convalida a EditItemTemplate s DataList

Durante la creazione di form di immissione dati, è importante che gli utenti immettono tutti i campi obbligatori e che tutti gli input specificati sono valori validi, formattata correttamente. Per garantire che un input dell'utente s siano validi, ASP.NET fornisce cinque controlli di convalida incorporate che sono progettati per convalidare il valore di un singolo controllo Web input:

- [RequiredFieldValidator](https://msdn.microsoft.com/library/5hbw267h(VS.80).aspx) assicura che è stato fornito un valore
- [CompareValidator](https://msdn.microsoft.com/library/db330ayw(VS.80).aspx) convalida un valore rispetto a un altro valore di controllo Web o un valore costante o assicura che il formato del valore s sia valido per un tipo di dati specificato
- [RangeValidator](https://msdn.microsoft.com/library/f70d09xt.aspx) garantisce che un valore sia entro un intervallo di valori
- [RegularExpressionValidator](https://msdn.microsoft.com/library/eahwtc9e.aspx) convalida di un valore rispetto a un [espressioni regolari](http://en.wikipedia.org/wiki/Regular_expression)
- [CustomValidator](https://msdn.microsoft.com/library/9eee01cx(VS.80).aspx) convalida un valore rispetto a un metodo personalizzato definito dall'utente

Per altre informazioni su questi cinque controlli fare riferimento al [aggiunta di controlli di convalida per la modifica e inserimento di interfacce](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) esercitazione, oppure consultare il [sezione relativa ai controlli di convalida](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/validation/default.aspx) del [Esercitazioni introduttive ASP.NET](https://quickstarts.asp.net).

Per questa esercitazione è necessario usare un RequiredFieldValidator per garantire che è stato fornito un valore per il nome del prodotto e un controllo CompareValidator per garantire che il prezzo immesso ha un valore maggiore o uguale a 0 e verrà visualizzato in un formato di valuta validi.

> [!NOTE]
> Mentre ASP.NET 1.x ha questi stessi controlli di cinque convalida, ASP.NET 2.0 ha aggiunto una serie di miglioramenti, principale due script lato client il supporto per i browser oltre a Internet Explorer e la possibilità di controlli di convalida di partizione in una pagina in gruppi di convalida. Per altre informazioni sulle nuove funzionalità di controllo di convalida in 2.0, consultare [Sezionando i controlli di convalida in ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx).


Let s iniziare aggiungendo i controlli di convalida necessari per il controllo DataList s `EditItemTemplate`. Questa attività può essere eseguita tramite la finestra di progettazione facendo clic sul collegamento di modifica modelli dello smart tag s DataList o tramite la sintassi dichiarativa. Consentire il passaggio s tramite il processo usando l'opzione di modifica modelli dalla visualizzazione progettazione. Dopo aver scelto di modifica di DataList 1!s `EditItemTemplate`, aggiungere un controllo RequiredFieldValidator trascinandolo dalla casella degli strumenti all'interfaccia di modifica del modello, posizionandolo dopo il `ProductName` nella casella di testo.


[![Aggiungere un RequiredFieldValidator EditItemTemplate dopo la casella di testo ProductName](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image8.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image7.png)

**Figura 3**: aggiungere un RequiredFieldValidator per il `EditItemTemplate After` le `ProductName` nella casella di testo ([fare clic per visualizzare l'immagine con dimensioni normali](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image9.png))


Tutti i controlli di convalida di lavoro per la convalida dell'input di un singolo controllo Web ASP.NET. Di conseguenza, dobbiamo indicare che il controllo RequiredFieldValidator appena aggiunto dovrebbe convalidato in base il `ProductName` casella di testo; questa operazione viene eseguita impostando il controllo di convalida s [ `ControlToValidate` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.controltovalidate(VS.80).aspx) per il `ID` di il controllo Web appropriato (`ProductName`, in questo caso). Successivamente, impostare il [ `ErrorMessage` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.errormessage(VS.80).aspx) per specificare il nome del prodotto s e il [ `Text` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.text(VS.80).aspx) a \*. Il `Text` valore della proprietà, se fornito, è il testo visualizzato dal controllo di convalida se la convalida non riesce. Il `ErrorMessage` valore della proprietà, è necessario, viene utilizzata dal controllo di controllo ValidationSummary; se il `Text` valore della proprietà viene omessa, il `ErrorMessage` valore della proprietà viene visualizzato dal controllo di convalida nell'input non valido.

Dopo aver impostato queste tre proprietà di RequiredFieldValidator, la schermata dovrebbe essere simile alla figura 4.


[![Impostare le proprietà di testo, un messaggio di errore e un RequiredFieldValidator s ControlToValidate](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image11.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image10.png)

**Figura 4**: impostare la s RequiredFieldValidator `ControlToValidate`, `ErrorMessage`, e `Text` delle proprietà ([fare clic per visualizzare l'immagine con dimensioni normali](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image12.png))


Con RequiredFieldValidator aggiunto per il `EditItemTemplate`, resta per aggiungere la convalida necessaria per il prezzo del prodotto s nella casella di testo. Poiché il `UnitPrice` è facoltativo quando si modifica un record, non abbiamo t è necessario aggiungere un RequiredFieldValidator. , Tuttavia, dobbiamo aggiungere un controllo CompareValidator per assicurarsi che il `UnitPrice`, se fornito, viene correttamente formattato come valuta ed è maggiore o uguale a 0.

Aggiungere il controllo CompareValidator nel `EditItemTemplate` e impostare relativi `ControlToValidate` proprietà `UnitPrice`, la relativa `ErrorMessage` proprietà per il prezzo deve essere maggiore o uguale a zero e non può includere il simbolo di valuta e il relativo `Text` proprietà \*. Per indicare che il `UnitPrice` valore deve essere maggiore o uguale a 0, impostare la s CompareValidator [ `Operator` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.operator(VS.80).aspx) al `GreaterThanEqual`, la relativa [ `ValueToCompare` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.valuetocompare(VS.80).aspx) su 0, e relativi [ `Type` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basecomparevalidator.type.aspx) a `Currency`.

Dopo aver aggiunto queste due controlli di convalida, DataList s `EditItemTemplate` sintassi dichiarativa per s dovrebbe essere simile al seguente:


[!code-aspx[Main](adding-validation-controls-to-the-datalist-s-editing-interface-cs/samples/sample1.aspx)]

Dopo aver apportato queste modifiche, aprire la pagina in un browser. Se si tenta di omettere il nome o immettere un valore del prezzo non valido durante la modifica di un prodotto, viene visualizzato un asterisco accanto alla casella di testo. Come illustrato nella figura 5, un valore di prezzo che include il simbolo di valuta, ad esempio 19,95 dollari con acquisto viene considerato non valido. Le s CompareValidator `Currency` `Type` consente di separatori di cifre (ad esempio virgole o punti, a seconda delle impostazioni cultura) e un segno iniziale più o meno, a differenza *non* consentono un simbolo di valuta. Questo comportamento potrebbe perplex agli utenti come l'interfaccia di modifica attualmente viene eseguito il rendering di `UnitPrice` utilizzando il formato di valuta.


[![Viene visualizzato un asterisco accanto alle caselle di testo con Input non valido](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image14.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image13.png)

**Figura 5**: un asterisco viene visualizzata accanto alla caselle di testo con Input non valido ([fare clic per visualizzare l'immagine con dimensioni normali](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image15.png))


Durante il funzionamento della convalida come-è, l'utente deve rimuovere manualmente il simbolo di valuta, quando si modifica un record che non è accettabile. Inoltre, se vi sono validi gli input di modifica dell'interfaccia né l'aggiornamento né Annulla pulsanti, quando selezionato, richiama un postback. In teoria, il pulsante Annulla restituirebbe DataList allo stato pre-editing indipendentemente dalla validità degli input utente s. Inoltre, è necessario verificare che i dati della pagina s siano validi prima di aggiornare le informazioni sul prodotto in DataList s `UpdateCommand` gestore eventi, come i controlli di convalida lato client per la logica può essere ignorata da utenti che usano browser si supporta JavaScript o sono il supporto disabilitato.

## <a name="removing-the-currency-symbol-from-the-edititemtemplate-s-unitprice-textbox"></a>Rimuovere il simbolo di valuta dalla casella di testo UnitPrice s EditItemTemplate

Quando si utilizza la s CompareValidator `Currency``Type`, l'input da convalidare non deve includere i simboli di valuta. La presenza di tali simboli fa sì che il controllo CompareValidator contrassegnare l'input come non valida. Tuttavia, l'interfaccia di modifica attualmente include un simbolo di valuta nel `UnitPrice` nella casella di testo, vale a dire l'utente deve rimuovere il simbolo di valuta in modo esplicito prima di salvare le modifiche. Per risolvere questo problema sono disponibili tre opzioni:

1. Configurare il `EditItemTemplate` in modo che il `UnitPrice` valore nella casella di testo non è formattato come una valuta.
2. Consente all'utente di immettere un simbolo di valuta, rimozione controllo CompareValidator e sostituendolo con un RegularExpressionValidator che verifica la presenza di un valore di valuta formattato correttamente. In questo caso il problema è che l'espressione regolare per convalidare un valore di valuta non è semplice come controllo CompareValidator e richiede la scrittura di codice se si vuole incorporare impostazioni cultura.
3. Rimuovere completamente il controllo di convalida e si basano sulla logica di convalida sul lato server personalizzato nel controllo GridView s `RowUpdating` gestore dell'evento.

Let s go con l'opzione 1 per questa esercitazione. Attualmente le `UnitPrice` viene formattato come valore di valuta a causa dell'espressione di associazione dati per la casella di testo nel `EditItemTemplate`: `<%# Eval("UnitPrice", "{0:c}") %>`. Modifica il `Eval` dell'istruzione `Eval("UnitPrice", "{0:n2}")`, che formatta il risultato sotto forma di numero con due cifre di precisione. Ciò può essere eseguita direttamente tramite la sintassi dichiarativa o facendo clic sul collegamento Modifica DataBindings dal `UnitPrice` casella di testo in s DataList `EditItemTemplate`.

Con questa modifica, il prezzo formattato nell'interfaccia di modifica sono incluse le virgole come separatore di gruppo e un punto come separatore decimale, ma lascia disattivare il simbolo di valuta.

> [!NOTE]
> Quando si rimuove il formato della valuta dall'interfaccia modificabile, utile inserire il simbolo di valuta come testo all'esterno di casella di testo. Questo funge da suggerimento per l'utente che non è necessario fornire il simbolo di valuta.


## <a name="fixing-the-cancel-button"></a>Correggere il pulsante Annulla

Per impostazione predefinita, i controlli di convalida Web generano JavaScript per eseguire la convalida sul lato client. Quando viene selezionato un pulsante, LinkButton e ImageButton, i controlli di convalida nella pagina vengono controllati sul lato client prima dell'esecuzione del postback. Se sono presenti dati non validi, viene annullato il postback. Per alcuni pulsanti, tuttavia, la validità dei dati potrebbe essere irrilevante; In tal caso, visto il postback annullato a causa di dati non validi è qualche difficoltà.

Il pulsante Annulla è un esempio di questo tipo. Si supponga che un utente immette i dati non validi, ad esempio il nome del prodotto s, omettendo quindi decide Mary t desidera salvare il prodotto dopo che tutti e preme il pulsante Annulla. Attualmente, il pulsante Annulla attiva i controlli di convalida della pagina, che segnalano che il nome del prodotto non è presente e impedire il postback. L'utente deve digitare un testo nel `ProductName` nella casella di testo solo per annullare il processo di modifica.

Per fortuna, il pulsante LinkButton e ImageButton hanno una [ `CausesValidation` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.causesvalidation.aspx) che può indicare se facendo clic sul pulsante deve avviare la logica di convalida (per impostazione predefinita `True`). Impostare la s pulsante Cancel `CausesValidation` proprietà `False`.

## <a name="ensuring-the-inputs-are-valid-in-the-updatecommand-event-handler"></a>Verificare gli input sono validi nel gestore dell'evento UpdateCommand

A causa di uno script lato client generato da controlli di convalida, se un utente immette un input non valido i controlli di convalida annullare qualsiasi postback avviato dal pulsante, LinkButton, o ImageButton controlli la cui proprietà `CausesValidation` sono proprietà `True` (il Per impostazione predefinita). Tuttavia, se un utente sta visitando un browser antiquato oppure a uno cui supporto per JavaScript è stata disabilitata, i controlli di convalida lato client non verranno eseguita.

Tutti i controlli di convalida ASP.NET ripetere la logica di convalida immediatamente dopo il postback e segnalare la validità generale degli input pagina s tramite il [ `Page.IsValid` proprietà](https://msdn.microsoft.com/library/system.web.ui.page.isvalid.aspx). Tuttavia, il flusso della pagina non viene interrotta o arrestato in qualsiasi modalità in base al valore di `Page.IsValid`. Gli sviluppatori è il compito del programmatore garantire che il `Page.IsValid` un valore della proprietà è `True` prima di procedere con il codice che presuppone valido i dati di input.

Se un utente ha JavaScript disabilitato, visita la pagina, consente di modificare un prodotto, immette un valore del prezzo di troppo costoso e fa clic sul pulsante di aggiornamento, la convalida lato client verrà ignorata e verrà verificare un postback. Durante il postback, ASP.NET pagina s `UpdateCommand` gestore dell'evento e viene generata un'eccezione durante il tentativo di analizzare troppo costosa da un `Decimal`. Poiché è disponibile la gestione delle eccezioni, tale eccezione verrà gestita correttamente, ma è possibile impedire che i dati non validi sfuggono in primo luogo solo continua con il `UpdateCommand` gestore dell'evento se `Page.IsValid` ha il valore `True`.

Aggiungere il codice seguente all'inizio della `UpdateCommand` gestore eventi, immediatamente prima di `Try` blocco:


[!code-csharp[Main](adding-validation-controls-to-the-datalist-s-editing-interface-cs/samples/sample2.cs)]

Grazie a questa aggiunta, il prodotto tenterà di essere aggiornata solo se i dati inviati sono validi. La maggior parte degli utenti ha vinto t in grado di dati non validi a causa di convalida i controlli lato client script di postback, ma gli utenti che usano browser si supporta JavaScript o che dispongono di JavaScript supportano disabilitato, ignorare i controlli sul lato client e inviare dati non validi.

> [!NOTE]
> Il lettore astuto ricorderete che durante l'aggiornamento dei dati con il controllo GridView, abbiamo t è necessario verificare in modo esplicito il `Page.IsValid` proprietà nella classe code-behind pagina s. Infatti GridView consulta il `Page.IsValid` Stati Uniti e solo procede con l'aggiornamento solo se viene restituito un valore di proprietà `True`.


## <a name="step-3-summarizing-data-entry-problems"></a>Passaggio 3: Riepilogo dei problemi nell'immissione dati

Oltre ai controlli di cinque convalida, ASP.NET include la [controllo di controllo ValidationSummary](https://msdn.microsoft.com/library/f9h59855(VS.80).aspx), che consente di visualizzare il `ErrorMessage` s tali controlli di convalida che ha rilevato dati non validi. Questi dati di riepilogo possono essere visualizzati come testo della pagina web o tramite una finestra di messaggio modale, lato client. Let s migliorare questa esercitazione per includere un messagebox sul lato client, il riepilogo di eventuali problemi di convalida.

A tale scopo, trascinare un controllo di controllo ValidationSummary dalla casella degli strumenti nella finestra di progettazione. Il percorso di t l di controllo ValidationSummary davvero è importante, poiché si ri prevede di configurare in modo da essere visualizzato solo il riepilogo come un oggetto messagebox. Dopo aver aggiunto il controllo, impostare relativi [ `ShowSummary` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showsummary(VS.80).aspx) al `False` e il relativo [ `ShowMessageBox` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showmessagebox(VS.80).aspx) a `True`. Grazie a questa aggiunta, eventuali errori di convalida sono riepilogati in messagebox lato client (vedere la figura 6).


[![Gli errori di convalida sono riepilogati in Messagebox lato Client](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image17.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image16.png)

**Figura 6**: di errori di convalida sono riepilogati in Messagebox lato Client ([fare clic per visualizzare l'immagine con dimensioni normali](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image18.png))


## <a name="summary"></a>Riepilogo

In questa esercitazione è stato illustrato come ridurre la probabilità di eccezioni usando i controlli di convalida in modo proattivo assicurarsi che l'input degli utenti siano validi prima di provare a usarli nel flusso di lavoro ad aggiornamento. ASP.NET fornisce cinque controlli Web di convalida che sono progettati per controllare un determinato Web controllano l'input s e segnalino la validità di input s. In questa esercitazione viene usato due di questi cinque controlli RequiredFieldValidator e controllo CompareValidator per assicurarsi che sia stato specificato il nome del prodotto s e che il prezzo ha un formato di valuta con un valore maggiore o uguale a zero.

Aggiunta di controlli di convalida al s DataList modifica interfaccia è semplice quanto trascinandoli nel `EditItemTemplate` dalla casella degli strumenti e impostando un numero limitato di proprietà. Per impostazione predefinita, i controlli di convalida generano automaticamente script di convalida lato client. ma offrono anche la convalida sul lato server durante il postback, archiviando il risultato cumulativo nel `Page.IsValid` proprietà. Per ignorare la convalida lato client quando viene selezionato un pulsante, LinkButton e ImageButton, impostare il pulsante 1!s `CausesValidation` proprietà `False`. Inoltre, prima di eseguire qualsiasi attività con i dati inviati durante il postback, assicurarsi che il `Page.IsValid` restituisce proprietà `True`.

Tutti DataList modifica esercitazioni si va esaminato finora è stato molto semplice modifica interfacce una casella di testo per il nome del prodotto s e un altro per il prezzo. L'interfaccia di modifica, tuttavia, può contenere una combinazione di controlli Web diversi, ad esempio controlli DropDownList, calendari, pulsanti di opzione, caselle di controllo e così via. Nell'esercitazione successiva esamineremo la creazione di un'interfaccia che usa un'ampia gamma di controlli Web.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. I revisori per questa esercitazione sono Liz Shulok, Dennis Patterson e Ken Pespisa. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](handling-bll-and-dal-level-exceptions-cs.md)
> [Successivo](customizing-the-datalist-s-editing-interface-cs.md)
