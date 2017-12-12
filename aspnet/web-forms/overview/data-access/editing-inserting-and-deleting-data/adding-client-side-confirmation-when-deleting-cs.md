---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-cs
title: Aggiunta di conferma dal lato Client durante l'eliminazione (c#) | Documenti Microsoft
author: rick-anderson
description: "In interfacce che abbiamo creato fino a questo punto, un utente può eliminare accidentalmente dati facendo clic sul pulsante Elimina quando si intende fare clic sul pulsante Modifica. In questo t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: f6e2a12a-2b5e-48fd-8db3-1e94a500c19a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-cs
msc.type: authoredcontent
ms.openlocfilehash: c5e8ee76224a48d3132597016b81099bd70a1776
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="adding-client-side-confirmation-when-deleting-c"></a>Aggiunta di conferma dal lato Client durante l'eliminazione (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare App di esempio](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_22_CS.exe) o [Scarica il PDF](adding-client-side-confirmation-when-deleting-cs/_static/datatutorial22cs1.pdf)

> In interfacce che abbiamo creato fino a questo punto, un utente può eliminare accidentalmente dati facendo clic sul pulsante Elimina quando si intende fare clic sul pulsante Modifica. In questa esercitazione si aggiungerà una finestra di dialogo di conferma dal lato client che viene visualizzato quando si fa clic sul pulsante Elimina.


## <a name="introduction"></a>Introduzione

Tramite le diverse esercitazioni precedenti abbiamo ve esaminare l'utilizzo dei controlli Web per l'architettura dell'applicazione e ObjectDataSource insieme per fornire l'inserimento, modifica ed eliminazione di funzionalità. L'eliminazione di interfacce è ve esaminate finora sono state composto da un'operazione di eliminazione pulsante che, quando selezionato, provoca un postback e richiama il s ObjectDataSource `Delete()` metodo. Il `Delete()` metodo richiama quindi il metodo configurato dal livello di logica di Business, che propaga la chiamata al livello di accesso ai dati, il rilascio effettivo `DELETE` istruzione per il database.

Durante questa interfaccia utente consente visitatori eliminare i record da un controllo GridView, DetailsView o FormView, manca una sorta di conferma quando l'utente fa clic sul pulsante Elimina. Se accidentalmente un utente fa clic sul pulsante Elimina quando si intende fare clic su Modifica, il record che sono solo per l'aggiornamento verrà invece eliminato. Per evitare questo problema, in questa esercitazione che si aggiungerà una finestra di dialogo di conferma dal lato client che viene visualizzato quando si fa clic sul pulsante Elimina.

Il codice JavaScript `confirm(string)` funzione consente di visualizzare il relativo parametro di input di stringa come il testo all'interno di una finestra di dialogo modale che dotato di due pulsanti - OK e Annulla (vedere la figura 1). Il `confirm(string)` funzione restituisce un valore booleano a seconda di quale pulsante (`true`, se l'utente fa clic su OK, e `false` se si fa clic su Annulla).


![Il metodo di confirm(string) JavaScript Visualizza modale, Messagebox sul lato Client](adding-client-side-confirmation-when-deleting-cs/_static/image1.png)

**Figura 1**: JavaScript `confirm(string)` metodo visualizza una finestra di messaggio modale, sul lato Client


Durante l'invio di un form, se un valore di `false` viene restituito da un gestore eventi lato client, quindi è stata annullata l'invio del modulo. Utilizzare questa funzionalità, è possibile disporre Delete pulsante s lato client `onclick` gestore evento restituisce il valore di una chiamata a `confirm("Are you sure you want to delete this product?")`. Se l'utente fa clic su Annulla, `confirm(string)` restituirà false, in tal modo che causa l'invio del modulo annullare. Con alcun postback, è possibile eliminare il prodotto è stato fatto clic con pulsante Elimina vinto t. Se, tuttavia, l'utente fa clic su OK nella finestra di dialogo di conferma, il postback continueranno esponenziale e verrà eliminato il prodotto. Consultare [s utilizzando JavaScript `confirm()` metodo per l'invio del modulo controllo](http://www.webreference.com/programming/javascript/confirm/) per ulteriori informazioni su questa tecnica.

Aggiunta di script sul lato client necessario differisce leggermente se usano i modelli rispetto all'utilizzo di un CommandField. Pertanto, in questa esercitazione verranno esaminati esempio GridView sia FormView.

> [!NOTE]
> Utilizzando tecniche di conferma dal lato client, come quelle descritte in questa esercitazione si presuppone che gli utenti sono visita con i browser che supportano JavaScript e che abbiano JavaScript abilitato. Se uno di questi presupposti non vengono soddisfatte per un utente specifico, fare clic sul pulsante Elimina immediatamente causa un postback (non viene visualizzata una finestra di messaggio di conferma).


## <a name="step-1-creating-a-formview-that-supports-deletion"></a>Passaggio 1: Creazione di un controllo FormView che supporta l'eliminazione

Per iniziare, aggiungere un controllo FormView per il `ConfirmationOnDelete.aspx` nella pagina di `EditInsertDelete` cartella, l'associazione a un nuovo oggetto ObjectDataSource che inserisce nuovamente le informazioni sul prodotto tramite il `ProductsBLL` classe s `GetProducts()` metodo. Configurare inoltre ObjectDataSource in modo che il `ProductsBLL` classe s `DeleteProduct(productID)` metodo viene mappato al s ObjectDataSource `Delete()` metodo; verificare che le schede, INSERT e UPDATE, elenchi a discesa sono impostati su (nessuno). Infine, controllare la casella di controllo Abilita Paging nello smart tag s FormView.

Dopo questi passaggi, il nuovo ObjectDataSource s dichiarativo sarà simile al seguente:


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample1.aspx)]

Come negli esempi precedenti che non è stata utilizzata la concorrenza ottimistica, dedicare alcuni minuti per cancellare i dispositivi ObjectDataSource `OldValuesParameterFormatString` proprietà.

Poiché è stato associato a un controllo ObjectDataSource che supporta solo l'eliminazione, s FormView `ItemTemplate` offre solo il pulsante Elimina mancante i pulsanti di New e Update. FormView s dichiarativo, tuttavia, include un superfluo `EditItemTemplate` e `InsertItemTemplate`, che può essere rimosso. Dedicare alcuni minuti per personalizzare il `ItemTemplate` in modo che sia illustrato solo un subset del prodotto campi dati. Si va configurato miei per visualizzare il nome del prodotto s in un `<h3>` titolo sopra i nomi di categoria e fornitore (con il pulsante Elimina).


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample2.aspx)]

Con queste modifiche, è disponibile una pagina web completamente funzionale che consente agli utenti di passare tra i prodotti uno alla volta, con la possibilità di eliminare un prodotto, facendo semplicemente clic sul pulsante Elimina. Figura 2 mostra una schermata dello stato di avanzamento finora quando viene visualizzato tramite un browser.


[![FormView mostra le informazioni su un singolo prodotto](adding-client-side-confirmation-when-deleting-cs/_static/image3.png)](adding-client-side-confirmation-when-deleting-cs/_static/image2.png)

**Figura 2**: il controllo FormView mostra informazioni su un singolo prodotto ([fare clic per visualizzare l'immagine ingrandita](adding-client-side-confirmation-when-deleting-cs/_static/image4.png))


## <a name="step-2-calling-the-confirmstring-function-from-the-delete-buttons-client-side-onclick-event"></a>Passaggio 2: Chiamata di confirm(string) funzione da onclick di eliminare i pulsanti sul lato Client evento

Con il controllo FormView creato, il passaggio finale consiste nel configurare il pulsante Elimina tali che, se si s selezionato per il visitatore, JavaScript `confirm(string)` funzione viene richiamata. Aggiunta di script sul lato client a un pulsante, LinkButton o ImageButton s lato client `onclick` evento può essere eseguito tramite l'utilizzo del `OnClientClick property`, che è una novità di ASP.NET 2.0. Poiché si vuole che il valore di `confirm(string)` restituito di funzione, è sufficiente impostare questa proprietà su:`return confirm('Are you certain that you want to delete this product?');`

Dopo questa modifica la sintassi dichiarativa di eliminare LinkButton s dovrebbe essere simile al seguente:


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample3.aspx)]

Sono disponibili tutte le che s è! Figura 3 mostra una schermata di questa conferma in azione. Fare clic sul pulsante Elimina viene visualizzata la finestra di dialogo di conferma. Se l'utente fa clic su Annulla, il prodotto non viene eliminato il postback è stato annullato. Se, tuttavia, l'utente fa clic su OK, il postback continua e s ObjectDataSource `Delete()` metodo viene richiamato, che si conclude il record di database da eliminare.

> [!NOTE]
> La stringa passata nel `confirm(string)` funzione JavaScript è delimitata da apostrofi (anziché tra virgolette). In JavaScript, le stringhe possono essere delimitate da un carattere. Utilizziamo apostrofi qui in modo che i delimitatori per la stringa passata in `confirm(string)` non introdurre un'ambiguità con i delimitatori utilizzati per il `OnClientClick` valore della proprietà.


[![Un messaggio di conferma è ora visualizzato quando il pulsante Elimina](adding-client-side-confirmation-when-deleting-cs/_static/image6.png)](adding-client-side-confirmation-when-deleting-cs/_static/image5.png)

**Figura 3**: conferma A viene ora visualizzato quando il pulsante Elimina ([fare clic per visualizzare l'immagine ingrandita](adding-client-side-confirmation-when-deleting-cs/_static/image7.png))


## <a name="step-3-configuring-the-onclientclick-property-for-the-delete-button-in-a-commandfield"></a>Passaggio 3: Configurazione di proprietà OnClientClick per il pulsante Elimina in un CommandField

Quando si lavora con un pulsante, LinkButton o ImageButton direttamente in un modello, può essere associato un finestra di dialogo di conferma, configurando la `OnClientClick` proprietà per restituire i risultati del codice JavaScript `confirm(string)` (funzione). Tuttavia, non dispone di CommandField, che aggiunge un campo di pulsanti di eliminazione a un controllo GridView o DetailsView - un `OnClientClick` proprietà che è possibile impostare in modo dichiarativo. In alternativa, è necessario a livello di codice fare riferimento a pulsante Elimina in s GridView o DetailsView appropriato `DataBound` gestore dell'evento e quindi impostare relativo `OnClientClick` proprietà non esiste.

> [!NOTE]
> Quando si imposta il pulsante Elimina s `OnClientClick` proprietà appropriata `DataBound` gestore eventi, è disponibile l'accesso per i dati associati al record corrente. Ciò significa che è possibile estendere il messaggio di conferma per includere i dettagli relativi al record specifico, ad esempio, "Sei si desidera eliminare il prodotto Chai?" Queste attività di personalizzazione è anche possibile nei modelli utilizzando la sintassi di associazione dati.


Impostazione di procedure consigliate di `OnClientClick` proprietà per i pulsanti Delete in un CommandField, s consentono di aggiungere un controllo GridView alla pagina. Configurare il controllo GridView. per utilizzare lo stesso controllo ObjectDataSource che utilizza il controllo FormView. Limitare anche i dispositivi di GridView BoundField per includere solo il nome del prodotto s, categoria e fornitore. Infine, controllare la casella di controllo Abilita eliminazione dallo smart tag s di GridView. Verrà aggiunta una CommandField al s GridView `Columns` insieme con il relativo `ShowDeleteButton` proprietà impostata su `true`.

Dopo aver apportato queste modifiche, il markup dichiarativo di GridView s dovrebbe essere simile al seguente:


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample4.aspx)]

Il CommandField contiene una sola istanza di eliminare LinkButton che è possibile accedere a livello di codice da s GridView `RowDataBound` gestore dell'evento. Quando si fa riferimento, è possibile impostare il relativo `OnClientClick` proprietà conseguenza. Creare un gestore eventi per il `RowDataBound` eventi utilizzando il codice seguente:


[!code-csharp[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample5.cs)]

Questo gestore eventi funziona con le righe di dati (quelli con il pulsante Elimina) e inizia a livello di codice facendo clic su pulsante Elimina. In generale, usare il modello seguente:


[!code-csharp[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample6.cs)]

*ButtonType* è il tipo di pulsante utilizzato da CommandField - Button, LinkButton o ImageButton. Per impostazione predefinita, il CommandField Usa LinkButton, ma può essere personalizzata tramite s CommandField `ButtonType property`. Il *commandFieldIndex* è l'indice ordinale del CommandField elementi GridView `Columns` insieme, mentre il *controlIndex* è l'indice del pulsante Elimina elementi CommandField `Controls` insieme. Il *controlIndex* valore dipende dalla posizione pulsante s rispetto agli altri pulsanti nel CommandField. Se, ad esempio, il solo pulsante visualizzato nel CommandField è il pulsante Elimina, utilizzare un indice 0. Se, tuttavia, non esiste un pulsante di modifica che precede il pulsante Elimina, usare un indice di 2. Il motivo viene utilizzato un indice di 2 è che i due controlli vengono aggiunti i CommandField prima il pulsante Elimina: pulsante Modifica e LiteralControl che s consente di aggiungere alcuni spazi tra i pulsanti Modifica e l'eliminazione.

Per questo particolare esempio, il CommandField utilizza LinkButton e, in campo più a sinistra ha un *commandFieldIndex* pari a 0. Poiché sono presenti altri pulsanti non ma sul pulsante Elimina il CommandField, utilizziamo un *controlIndex* pari a 0.

Dopo aver fatto riferimento sul pulsante Elimina il CommandField, è successivamente possibile acquisire informazioni sul prodotto associato alla riga corrente di GridView. Infine, viene impostato il pulsante Elimina s `OnClientClick` proprietà JavaScript appropriato, che include il nome del prodotto s. Poiché la stringa JavaScript passato il `confirm(string)` funzione delimitata utilizzando apostrofi è necessario escape qualsiasi apostrofi visualizzati all'interno del nome di prodotto s. In particolare, qualsiasi apostrofi nel nome del prodotto s sono preceduti da "`\'`".

Con queste modifiche completate, fare clic sul pulsante Elimina in GridView Visualizza una finestra di dialogo di conferma personalizzata casella (vedere Figura 4). Come con il messagebox conferma dal FormView, se l'utente fa clic su Annulla il postback viene annullato, impedendo in tal modo l'eliminazione in corso.

> [!NOTE]
> Questa tecnica può essere usata anche per accedere a livello di codice al pulsante di eliminazione in CommandField in un controllo DetailsView. Per il controllo DetailsView, tuttavia, d crei un gestore eventi per il `DataBound` evento, poiché non dispone di DetailsView un `RowDataBound` evento.


[![Fare clic sul pulsante di eliminazione s GridView Visualizza una finestra di dialogo di conferma personalizzata](adding-client-side-confirmation-when-deleting-cs/_static/image9.png)](adding-client-side-confirmation-when-deleting-cs/_static/image8.png)

**Figura 4**: fare clic sul pulsante Elimina s GridView Visualizza una finestra di dialogo di conferma personalizzato ([fare clic per visualizzare l'immagine ingrandita](adding-client-side-confirmation-when-deleting-cs/_static/image10.png))


## <a name="using-templatefields"></a>Utilizzando TemplateFields

Uno degli svantaggi del CommandField è che i relativi pulsanti devono essere accessibili mediante l'indicizzazione e che l'oggetto risultante deve eseguire il cast al tipo di pulsante appropriato (Button, LinkButton o ImageButton). Utilizzo di "numeri chiave" e tipi a livello di codice invita i problemi che non possono essere individuati fino al runtime. Ad esempio, se si o un altro sviluppatore, aggiunge nuovi pulsanti per la CommandField in un momento successivo (ad esempio un pulsante di modifica) o le modifiche di `ButtonType` proprietà, il codice esistente verrà comunque compilata senza errori, ma visita la pagina potrebbe causare un'eccezione o un comportamento imprevisto, a seconda della modalità di scrittura del codice e le modifiche apportate.

Un approccio alternativo consiste nel convertire s GridView e DetailsView CommandFields in TemplateFields. Verrà generato un TemplateField con un `ItemTemplate` che ha un LinkButton (pulsante o ImageButton) per ciascuno di essi il CommandField. Questi pulsanti `OnClientClick` proprietà possono essere assegnate in modo dichiarativo, come è illustrato con il controllo FormView, oppure è possibile accedere a livello di codice in appropriata `DataBound` gestore eventi usando il modello seguente:


[!code-csharp[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample7.cs)]

Dove *controlID* è il valore del pulsante s `ID` proprietà. Questo modello richiede ancora un tipo a livello di codice per il cast, Elimina la necessità per l'indicizzazione, consentendo il layout modificare senza genera un errore di runtime.

## <a name="summary"></a>Riepilogo

Il codice JavaScript `confirm(string)` funzione è una tecnica di uso comune per il controllo del flusso di lavoro invio modulo. Quando viene eseguita, la funzione Visualizza una finestra di dialogo modale, sul lato client che include i due pulsanti OK e Annulla. Se l'utente fa clic su OK, il `confirm(string)` risultato della funzione `true`; facendo clic su Annulla restituisce `false`. Questa funzionalità, insieme a un comportamento di browser s annullare l'invio di un form un gestore eventi durante il processo di invio restituisce `false`, può essere utilizzato per visualizzare una finestra di messaggio di conferma quando si elimina un record.

Il `confirm(string)` funzione può essere associata a un pulsante Web s controllo lato client `onclick` gestore eventi tramite il controllo s `OnClientClick` proprietà. Quando si lavora con il pulsante Elimina in un modello, in uno dei modelli FormView s o in un TemplateField nel controllo DetailsView o GridView - questa proprietà può essere impostata in modo dichiarativo o a livello di programmazione, come illustrato in questa esercitazione.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

>[!div class="step-by-step"]
[Precedente](implementing-optimistic-concurrency-cs.md)
[Successivo](limiting-data-modification-functionality-based-on-the-user-cs.md)
