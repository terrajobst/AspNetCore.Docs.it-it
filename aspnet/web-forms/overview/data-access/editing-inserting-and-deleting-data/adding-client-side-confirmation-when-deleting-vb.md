---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-vb
title: Aggiunta di una conferma lato Client durante l'eliminazione (VB) | Microsoft Docs
author: rick-anderson
description: Nelle interfacce che abbiamo creato finora, un utente può eliminare accidentalmente dei dati facendo clic sul pulsante Elimina quando si intende fare clic sul pulsante Modifica. In questo t...
ms.author: aspnetcontent
ms.date: 07/17/2006
ms.assetid: 6331e02e-c465-4cdf-bd3f-f07680c289d6
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-vb
msc.type: authoredcontent
ms.openlocfilehash: 2318018b20841038ebb7a6e4900cf9849057d941
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802494"
---
<a name="adding-client-side-confirmation-when-deleting-vb"></a>Aggiunta di una conferma lato Client durante l'eliminazione (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'App di esempio](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_22_VB.exe) o [Scarica il PDF](adding-client-side-confirmation-when-deleting-vb/_static/datatutorial22vb1.pdf)

> Nelle interfacce che abbiamo creato finora, un utente può eliminare accidentalmente dei dati facendo clic sul pulsante Elimina quando si intende fare clic sul pulsante Modifica. In questa esercitazione si aggiungerà una finestra di dialogo di conferma lato client che viene visualizzato quando si fa clic sul pulsante Elimina.


## <a name="introduction"></a>Introduzione

Tramite le varie esercitazioni precedenti abbiamo ve appreso come usare l'architettura delle applicazioni, ObjectDataSource e i controlli Web dei dati insieme per fornire l'inserimento, la modifica e l'eliminazione di funzionalità. L'eliminazione di interfacce è stato esaminato finora sono stati composto da un'operazione di eliminazione pulsante che, quando selezionato, causa un postback e richiama gli oggetti ObjectDataSource `Delete()` (metodo). Il `Delete()` metodo richiama quindi il metodo configurato dal livello della logica di Business, che propaga la chiamata al livello di accesso ai dati, emissione effettiva `DELETE` istruzione per il database.

Anche se questa interfaccia utente consente visitatori eliminare i record tra i controlli GridView, DetailsView e FormView, non l'ha una sorta di conferma quando l'utente fa clic sul pulsante Elimina. Se un utente fa clic accidentalmente il pulsante di eliminazione quando si intende fare clic su Modifica, verrà eliminato invece il record che si intende aggiornare. Per evitare ciò, in questa esercitazione si aggiungerà una finestra di dialogo di conferma lato client che viene visualizzato quando si fa clic sul pulsante Elimina.

Il codice JavaScript `confirm(string)` funzione consente di visualizzare il parametro di input di stringa come testo all'interno di una finestra di dialogo modale che è dotato di due pulsanti - OK e Cancel (vedere la figura 1). Il `confirm(string)` funzione restituisce un valore booleano in base alla quale pulsante (`true`, se l'utente fa clic su OK, e `false` se si fa clic su Annulla).


![Il confirm(string) JavaScript (metodo) Visualizza una finestra modale, Messagebox lato Client](adding-client-side-confirmation-when-deleting-vb/_static/image1.png)

**Figura 1**: il codice JavaScript `confirm(string)` metodo visualizza una finestra di messaggio modale, lato Client


Durante l'invio di un modulo, se il valore `false` viene restituito da un gestore eventi lato client, l'invio del form è stata annullata. Utilizzare questa funzionalità, possiamo avere Delete pulsante s lato client `onclick` gestore eventi restituire il valore di una chiamata a `confirm("Are you sure you want to delete this product?")`. Se l'utente fa clic su Annulla, `confirm(string)` restituirà false, in tal modo che causa l'invio del modulo annullare. Con alcun postback, eliminare il prodotto è stato fatto clic sul pulsante la cui eliminazione ha vinto t. Se, tuttavia, l'utente fa clic su OK nella finestra di dialogo di conferma, il postback continueranno maniera e verrà eliminato il prodotto. Consultare [uso di JavaScript s `confirm()` metodo per l'invio di Form di controllo](http://www.webreference.com/programming/javascript/confirm/) per altre informazioni su questa tecnica.

Aggiungere lo script lato client necessario differisce leggermente se usano i modelli rispetto all'uso di un CommandField. Pertanto, in questa esercitazione si esaminerà esempio un controllo FormView e GridView.

> [!NOTE]
> Usa tecniche di conferma lato client, come quelle descritte in questa esercitazione si presuppone che gli utenti vengono visitate con i browser che supportano JavaScript e che siano presenti JavaScript abilitato. Se uno di questi presupposti non sono true per un utente specifico, facendo clic sul pulsante Elimina immediatamente causa un postback (non la visualizzazione di una finestra di messaggio di conferma).


## <a name="step-1-creating-a-formview-that-supports-deletion"></a>Passaggio 1: Creazione di un controllo FormView che supporta l'eliminazione

Iniziare aggiungendo un controllo FormView per il `ConfirmationOnDelete.aspx` nella pagina la `EditInsertDelete` cartella, associarla a un nuovo oggetto ObjectDataSource che esegue il pull nuovamente le informazioni sul prodotto tramite il `ProductsBLL` classe s `GetProducts()` (metodo). Inoltre configurare ObjectDataSource in modo che il `ProductsBLL` classe s `DeleteProduct(productID)` metodo viene eseguito il mapping agli oggetti ObjectDataSource `Delete()` metodo; assicurarsi che le schede INSERT e UPDATE elenchi a discesa sono impostati su (nessuno). Infine, controllare la casella di controllo Attiva Paging nello smart tag FormView s.

Dopo questi passaggi, il nuovo markup dichiarativo di ObjectDataSource s avrà un aspetto simile al seguente:


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample1.aspx)]

Come negli esempi precedenti che non è stata utilizzata la concorrenza ottimistica, si consiglia di cancellare i dispositivi di ObjectDataSource `OldValuesParameterFormatString` proprietà.

Poiché è stato associato a un controllo ObjectDataSource che supporta solo l'eliminazione, la s FormView `ItemTemplate` offre solo il pulsante di eliminazione, se ne manca i pulsanti New e Update. FormView s dichiarativo, tuttavia, include una superflua `EditItemTemplate` e `InsertItemTemplate`, che può essere rimosso. Si consiglia di personalizzare il `ItemTemplate` in modo che sia illustra solo un subset del prodotto dei campi dati. Si va configurato data mining per visualizzare il nome del prodotto s in un `<h3>` intestazione sopra i nomi di categoria e fornitore (con il pulsante Elimina).


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample2.aspx)]

Con queste modifiche, è disponibile una pagina web completamente funzionale che consente agli utenti di passare tra i prodotti uno alla volta, con la possibilità di eliminare un prodotto facendo semplicemente clic sul pulsante Elimina. Figura 2 mostra una cattura di schermata dello stato di avanzamento fino ad ora, quando viene visualizzato tramite un browser.


[![FormView mostra le informazioni su un solo prodotto](adding-client-side-confirmation-when-deleting-vb/_static/image3.png)](adding-client-side-confirmation-when-deleting-vb/_static/image2.png)

**Figura 2**: The FormView mostra informazioni su un singolo prodotto ([fare clic per visualizzare l'immagine con dimensioni normali](adding-client-side-confirmation-when-deleting-vb/_static/image4.png))


## <a name="step-2-calling-the-confirmstring-function-from-the-delete-buttons-client-side-onclick-event"></a>Passaggio 2: Chiamata confirm(string) funzione da onclick di eliminare i pulsanti lato Client evento

Con il controllo FormView creato, il passaggio finale consiste nel configurare il pulsante Elimina tali che, se si s fa clic con il visitatore, il codice JavaScript sul `confirm(string)` funzione viene richiamata. Aggiunta di script sul lato client a un pulsante, LinkButton e ImageButton s client-side `onclick` evento può essere eseguito tramite l'uso del `OnClientClick property`, che è una novità di ASP.NET 2.0. Poiché si vuole che il valore della `confirm(string)` restituito di funzione, è sufficiente impostare questa proprietà: `return confirm('Are you certain that you want to delete this product?');`

Dopo questa modifica, la sintassi dichiarativa s eliminare LinkButton dovrebbe essere simile:


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample3.aspx)]

Che s ne è a esso. Figura 3 mostra una schermata di questa conferma in azione. Facendo clic sul pulsante Elimina viene visualizzata la finestra di dialogo di conferma. Se l'utente fa clic su Annulla, viene annullato il postback e il prodotto non viene eliminato. Se, tuttavia, l'utente fa clic su OK, continua il postback e gli oggetti ObjectDataSource `Delete()` metodo viene richiamato, che si conclude con il record del database in corso l'eliminazione.

> [!NOTE]
> La stringa passata nel `confirm(string)` funzione JavaScript è delimitato da apostrofi (anziché le virgolette). In JavaScript, le stringhe possono essere delimitati da un carattere. Usiamo apostrofi qui in modo che i delimitatori per la stringa passati `confirm(string)` non introducono ambiguità con i delimitatori utilizzati per il `OnClientClick` valore della proprietà.


[![Un messaggio di conferma viene ora visualizzata quando facendo clic sul pulsante Elimina](adding-client-side-confirmation-when-deleting-vb/_static/image6.png)](adding-client-side-confirmation-when-deleting-vb/_static/image5.png)

**Figura 3**: una conferma viene ora visualizzata quando si fa clic su Delete ([fare clic per visualizzare l'immagine con dimensioni normali](adding-client-side-confirmation-when-deleting-vb/_static/image7.png))


## <a name="step-3-configuring-the-onclientclick-property-for-the-delete-button-in-a-commandfield"></a>Passaggio 3: Configurazione della proprietà OnClientClick per il pulsante Elimina in un CommandField

Quando si lavora con un pulsante, LinkButton e ImageButton direttamente in un modello, una finestra di dialogo di conferma può essere associata ad esso configurando semplicemente relativi `OnClientClick` proprietà per restituire i risultati del codice JavaScript `confirm(string)` (funzione). Tuttavia, non dispone di CommandField, che aggiunge un campo di pulsanti di eliminazione a un controllo GridView o DetailsView - un `OnClientClick` proprietà che può essere impostata in modo dichiarativo. In alternativa, è necessario a livello di codice fare riferimento a sul pulsante Elimina in s GridView o DetailsView appropriato `DataBound` gestore dell'evento e quindi impostare relativo `OnClientClick` proprietà non esiste.

> [!NOTE]
> Quando si imposta il pulsante Elimina s `OnClientClick` proprietà nelle rispettive caselle `DataBound` gestore eventi, è possibile accedere a dati è stati associati al record corrente. Ciò significa che è possibile estendere il messaggio di conferma per includere i dettagli del record specifico, ad esempio, "Sono certi di che voler eliminare il prodotto Chai?" Queste attività di personalizzazione è anche possibile nei modelli usando la sintassi di associazione dati.


Impostazione di procedure consigliate di `OnClientClick` proprietà per l'aziona Delete in una CommandField, s "Let" aggiungere un controllo GridView alla pagina. Configurare il controllo GridView per usare lo stesso controllo ObjectDataSource che utilizza il controllo FormView. Anche limitare i dispositivi di GridView BoundField per includere solo la s nome prodotto, categoria e fornitore. Infine, controllare la casella di controllo Abilita eliminazione nello smart tag s GridView. Verrà aggiunto un CommandField al s GridView `Columns` raccolta con il relativo `ShowDeleteButton` impostata su `true`.

Dopo aver apportato queste modifiche, il markup dichiarativo di GridView s dovrebbe essere simile al seguente:


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample4.aspx)]

Il CommandField contiene una singola istanza di LinkButton eliminazione che è possibile accedere a livello di codice da s GridView `RowDataBound` gestore dell'evento. Dopo aver fatto riferimento, è possibile impostare relativo `OnClientClick` proprietà conseguenza. Creare un gestore eventi per il `RowDataBound` eventi usando il codice seguente:


[!code-vb[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample5.vb)]

Questo gestore di evento funziona con le righe di dati (quelli che avrà il pulsante Elimina) e inizia a livello di codice facendo clic sul pulsante Elimina. In generale, usare il modello seguente:


[!code-vb[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample6.vb)]

*ButtonType* è il tipo di pulsante utilizzato da CommandField - pulsante, LinkButton e ImageButton. Per impostazione predefinita, il CommandField Usa i controlli LinkButton, ma può essere personalizzato tramite gli oggetti CommandField `ButtonType property`. Il *commandFieldIndex* è l'indice ordinale del CommandField entro la s GridView `Columns` insieme, mentre il *controlIndex* è l'indice del pulsante di eliminazione entro la s CommandField `Controls` raccolta. Il *controlIndex* valore dipende la posizione del pulsante s rispetto agli altri pulsanti nella CommandField. Ad esempio, se l'unico pulsante visualizzato nella finestra di CommandField è il pulsante di eliminazione, usare un indice pari a 0. Se, tuttavia, ci un pulsante di modifica che precede il pulsante di eliminazione, usare un indice di 2. Il motivo viene usato un indice pari a 2 è che vengono aggiunti due controlli per il CommandField prima del pulsante di eliminazione: il pulsante di modifica e LiteralControl tale s utilizzato per aggiungere alcuni spazi tra i pulsanti di modifica ed eliminazione.

Per questo particolare esempio, il CommandField Usa LinkButton e, in corso il campo più a sinistra ha un *commandFieldIndex* pari a 0. Poiché sono presenti sul pulsante Elimina nel CommandField ma non altri pulsanti, usiamo una *controlIndex* pari a 0.

Dopo aver fatto riferimento il pulsante Elimina nel CommandField, si ottiene quindi le informazioni sul prodotto associato alla riga corrente di GridView. Infine, impostiamo il pulsante Elimina s `OnClientClick` proprietà JavaScript appropriato, che include il nome del prodotto s. Poiché la stringa di JavaScript passato il `confirm(string)` funzione delimitata usando apostrofi è necessario eseguire l'escape qualsiasi apostrofi visualizzati all'interno del nome di prodotto s. In particolare, qualsiasi apostrofi nel nome del prodotto s sono preceduti da "`\'`".

Con queste modifiche completate, facendo clic sul pulsante Elimina in GridView Visualizza una finestra di dialogo di conferma personalizzata casella (vedere la figura 4). Come con il messagebox conferma di FormView, se l'utente fa clic su Annulla il postback è stato annullato, impedendo in tal modo l'eliminazione che si verifichi.

> [!NOTE]
> Questa tecnica può essere utilizzata anche per accedere a livello di codice al pulsante di eliminazione in CommandField in un controllo DetailsView. Per DetailsView, tuttavia, è il d creare un gestore eventi per il `DataBound` evento, poiché non esiste un DetailsView un `RowDataBound` evento.


[![Facendo clic sul pulsante Elimina s GridView Visualizza una finestra di dialogo di conferma personalizzata](adding-client-side-confirmation-when-deleting-vb/_static/image9.png)](adding-client-side-confirmation-when-deleting-vb/_static/image8.png)

**Figura 4**: facendo clic sul pulsante Elimina s GridView Visualizza una finestra di dialogo di conferma personalizzata ([fare clic per visualizzare l'immagine con dimensioni normali](adding-client-side-confirmation-when-deleting-vb/_static/image10.png))


## <a name="using-templatefields"></a>Uso di TemplateFields

Uno degli svantaggi del CommandField è che i pulsanti è possibile accedervi tramite l'indicizzazione e che l'oggetto risultante deve essere impostata sul tipo di pulsante appropriato (pulsante, LinkButton e ImageButton). Utilizzo di "numeri chiave" e tipi a livello di codice invita i problemi che non è possibile individuare fino al runtime. Ad esempio, se è o un altro sviluppatore, aggiunge nuovi pulsanti alla finestra di CommandField a un certo punto in futuro (ad esempio un pulsante di modifica) o le modifiche di `ButtonType` proprietà, il codice esistente verrà comunque compilato senza errori, ma visitando la pagina potrebbe causare un'eccezione o un comportamento imprevisto, a seconda del modo in cui è stato scritto il codice e le modifiche apportate.

Un approccio alternativo consiste nel convertire la s GridView e DetailsView CommandFields in TemplateFields. Verrà generato un TemplateField con un `ItemTemplate` che ha un LinkButton (o sul pulsante o ImageButton) per ciascun pulsante nel CommandField. Questi pulsanti `OnClientClick` delle proprietà possono essere assegnate in modo dichiarativo, come abbiamo visto con FormView o sono accessibili a livello di codice nelle rispettive caselle `DataBound` gestore eventi usando il modello seguente:


[!code-vb[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample7.vb)]

In cui *controlID* è il valore del pulsante s `ID` proprietà. Mentre questo modello richiede comunque un tipo a livello di codice per il cast, Elimina la necessità per l'indicizzazione, consentendo il layout modificare senza causando un errore di runtime.

## <a name="summary"></a>Riepilogo

Il codice JavaScript `confirm(string)` funzione è una tecnica comunemente utilizzata per il controllo del flusso di lavoro invio dei form. Quando viene eseguita, la funzione consente di visualizzare una finestra di dialogo modale, sul lato client, che include due pulsanti OK e Annulla. Se l'utente fa clic su OK, il `confirm(string)` funzione restituisce `true`; facendo clic su Annulla restituisce `false`. Questa funzionalità, insieme a un comportamento di browser s per annullare l'invio di un form se restituisce un gestore di eventi durante il processo di invio `false`, può essere utilizzato per visualizzare una finestra di messaggio di conferma quando si elimina un record.

Il `confirm(string)` funzione può essere associata a un controllo s Web pulsante client-side `onclick` gestore dell'evento tramite il controllo s `OnClientClick` proprietà. Quando si lavora con un pulsante Elimina in un modello: in uno dei modelli di FormView s o in un TemplateField nel controllo DetailsView o GridView - questa proprietà può essere impostata in modo dichiarativo o a livello di programmazione, come abbiamo visto in questa esercitazione.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Precedente](implementing-optimistic-concurrency-vb.md)
> [Successivo](limiting-data-modification-functionality-based-on-the-user-vb.md)
