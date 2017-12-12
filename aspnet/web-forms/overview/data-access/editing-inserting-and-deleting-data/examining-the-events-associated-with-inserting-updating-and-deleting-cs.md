---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs
title: Esaminare gli eventi associati di inserimento, aggiornamento ed eliminazione (c#) | Documenti Microsoft
author: rick-anderson
description: In questa esercitazione che si esamineranno usando gli eventi che si verificano prima, durante e dopo un inserimento, aggiornamento o eliminazione di un controllo Web di dati ASP.NET. W....
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: dab291a0-a8b5-46fa-9dd8-3d35b249201f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs
msc.type: authoredcontent
ms.openlocfilehash: 30f6ecef1a03153619df1b3ba4e709f3742c6927
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="examining-the-events-associated-with-inserting-updating-and-deleting-c"></a>Esaminare gli eventi associati all'inserimento, aggiornamento ed eliminazione (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare App di esempio](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_17_CS.exe) o [Scarica il PDF](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/datatutorial17cs1.pdf)

> In questa esercitazione che si esamineranno usando gli eventi che si verificano prima, durante e dopo un inserimento, aggiornamento o eliminazione di un controllo Web di dati ASP.NET. Inoltre, vedremo come personalizzare l'interfaccia di modifica per aggiornare solo un sottoinsieme dei campi di prodotto.


## <a name="introduction"></a>Introduzione

Quando si utilizza l'incorporate di inserimento, la modifica o eliminazione di funzionalità dei controlli GridView, DetailsView o FormView, un'ampia gamma di passaggi trascorrere quando l'utente finale completa il processo di aggiunta di un nuovo record o l'aggiornamento o eliminazione di un record esistente. Come accennato nel [esercitazione precedente](an-overview-of-inserting-updating-and-deleting-data-cs.md), quando una riga viene modificata in GridView con il pulsante Modifica viene sostituito dall'aggiornamento e annullamento pulsanti e l'attivazione di BoundField nelle caselle di testo. Dopo che l'utente finale Aggiorna i dati e sceglie di aggiornamento, durante il postback vengono eseguite le seguenti operazioni:

1. GridView popola ObjectDataSource `UpdateParameters` con campi di identificazione univoco del record modificato (tramite la `DataKeyNames` proprietà) insieme ai valori immessi dall'utente
2. GridView richiama ObjectDataSource `Update()` metodo, che a sua volta richiama il metodo appropriato nell'oggetto sottostante (`ProductsDAL.UpdateProduct`, in questa esercitazione precedente)
3. I dati sottostanti, che include ora le modifiche aggiornate, vengano riassociati al controllo GridView.

Durante questa sequenza di passaggi, generare un numero di eventi, consentono di creare gestori eventi per aggiungere una logica personalizzata in caso di necessità. Ad esempio, prima del passaggio 1, il controllo GridView `RowUpdating` viene generato l'evento. È possibile, annullare la richiesta di aggiornamento a questo punto, nel caso di errori di convalida. Quando il `Update()` metodo viene richiamato, ObjectDataSource `Updating` viene generato l'evento fornisce la possibilità di aggiungere o personalizzare i valori di qualsiasi di `UpdateParameters`. Dopo aver sottostante ObjectDataSource metodo dell'oggetto ha completato l'esecuzione, di ObjectDataSource `Updated` viene generato l'evento. Un gestore eventi per il `Updated` eventi è possono esaminare i dettagli sull'operazione di aggiornamento, ad esempio il numero di righe interessato e se si è verificata un'eccezione. Infine, dopo il passaggio 2, GridView `RowUpdated` viene generato l'evento, un gestore eventi per eventi può esaminare le informazioni aggiuntive sull'operazione di aggiornamento appena eseguite.

Figura 1 illustra questa serie di eventi e i passaggi durante l'aggiornamento di un controllo GridView. Lo schema di eventi nella figura 1 non è univoco per l'aggiornamento con un controllo GridView. Inserimento, aggiornamento o eliminazione di dati da GridView, DetailsView o FormView precipitates la stessa sequenza di eventi di pre e post-livelli per il controllo Web di dati e ObjectDataSource.


[![Una serie di pre- e attivare gli post-eventi durante l'aggiornamento dati in un controllo GridView.](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image2.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image1.png)

**Figura 1**: una serie di pre- e post-incendio quando l'aggiornamento dei dati degli eventi in GridView ([fare clic per visualizzare l'immagine ingrandita](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image3.png))


In questa esercitazione che si esamineranno utilizza questi eventi per estendere incorporate di inserimento, aggiornamento ed eliminazione di funzionalità dei dati ASP.NET Web controlla. Inoltre, vedremo come personalizzare l'interfaccia di modifica per aggiornare solo un sottoinsieme dei campi di prodotto.

## <a name="step-1-updating-a-productsproductnameandunitpricefields"></a>Passaggio 1: Aggiornare un prodotto`ProductName`e`UnitPrice`campi

In modificare interfacce dall'esercitazione precedente *tutti* campi prodotto che non sono in sola lettura deve essere incluso. Ad esempio se si dovesse per rimuovere un campo dal controllo GridView - `QuantityPerUnit` : quando l'aggiornamento dei dati di controllo Web di dati non imposta ObjectDataSource `QuantityPerUnit` `UpdateParameters` valore. ObjectDataSource passa quindi un `null` valore nel `UpdateProduct` metodo livello Business (LOGIC), che comporterebbe la modifica del record del database modificato `QuantityPerUnit` colonna un `NULL` valore. Analogamente, se un campo obbligatorio, ad esempio `ProductName`, viene rimosso dall'interfaccia di modifica, l'aggiornamento avrà esito negativo con un "*colonna 'ProductName' non ammette valori null*" eccezione. Il motivo di questo comportamento è stato perché ObjectDataSource è stato configurato per richiamare il `ProductsBLL` della classe `UpdateProduct` (metodo), che prevede un parametro di input per ognuno dei campi del prodotto. Pertanto, di ObjectDataSource `UpdateParameters` raccolta contenuto un parametro per ogni metodo parametri di input.

Se si desidera fornire un controllo Web che consente all'utente finale aggiornare solo un sottoinsieme dei campi di dati, quindi è necessario impostare a livello di codice la mancante `UpdateParameters` valori in ObjectDataSource `Updating` gestore dell'evento o di creare e chiamare un metodo BLL che è previsto solo un sottoinsieme dei campi. Esaminiamo questo approccio quest'ultimo.

In particolare, è possibile crearne una pagina che visualizza solo il `ProductName` e `UnitPrice` i campi in un controllo GridView. modificabile. Interfaccia per la modifica del controllo GridView. Questo consentirà solo all'utente di aggiornare i campi visualizzati due, `ProductName` e `UnitPrice`. Poiché questa modifica interfaccia fornisce solo un sottoinsieme dei campi di un prodotto, è necessario creare ObjectDataSource che utilizza il BLL esistente `UpdateProduct` (metodo) e i valori di campo di prodotto mancanti impostati a livello di codice nel relativo `Updating` evento gestore oppure è necessario creare un nuovo metodo BLL che prevede solo il subset di campi definiti in GridView. Per questa esercitazione, si utilizza l'opzione di quest'ultima e creare un overload di `UpdateProduct` (metodo), uno che accetta solo tre parametri di input: `productName`, `unitPrice`, e `productID`:


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample1.cs)]

Ad esempio originale `UpdateProduct` (metodo), questo overload viene avviato da un controllo per verificare se nel database con l'oggetto specificato è un prodotto `ProductID`. Se non viene restituito `false`, che indica che la richiesta per aggiornare le informazioni sul prodotto non è riuscita. In caso contrario viene aggiornato il record di prodotto esistente `ProductName` e `UnitPrice` campi conseguenza ed esegue il commit dell'aggiornamento chiamando il TableAdpater `Update()` metodo passando la `ProductsRow` istanza.

Con questa aggiunta per il nostro `ProductsBLL` (classe), si è pronti per creare l'interfaccia di GridView semplificata. Aprire il `DataModificationEvents.aspx` nel `EditInsertDelete` cartella e aggiungere un controllo GridView. Creare un nuovo oggetto ObjectDataSource e configurarlo in modo da utilizzare il `ProductsBLL` classe con il `Select()` mapping del metodo per `GetProducts` e il relativo `Update()` mapping del metodo per il `UpdateProduct` overload che accetta solo il `productName`, `unitPrice`, e `productID` i parametri di input. La figura 2 mostra la procedura guidata Crea origine dati durante il mapping di ObjectDataSource `Update()` metodo il `ProductsBLL` della nuova classe `UpdateProduct` overload del metodo.


[![Eseguire il mapping Update () metodo di ObjectDataSource per il nuovo Overload UpdateProduct](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image5.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image4.png)

**Figura 2**: eseguire il mapping di ObjectDataSource `Update()` il nuovo metodo `UpdateProduct` Overload ([fare clic per visualizzare l'immagine ingrandita](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image6.png))


Poiché questo esempio verrà inizialmente sufficiente la possibilità di modificare i dati, ma non di inserimento o eliminazione di record, è opportuno indicare esplicitamente che ObjectDataSource `Insert()` e `Delete()` non è possibile eseguire il mapping a uno dei metodi di `ProductsBLL` metodi della classe accedendo alle schede delle INSERT e DELETE e scegliendo (nessuno) dall'elenco a discesa.


[![Scegliere (nessuno) nell'elenco di riepilogo a discesa per l'INSERT e DELETE schede](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image8.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image7.png)

**Figura 3**: (nessuno) dal riepilogo per l'inserimento e schede eliminare ([fare clic per visualizzare l'immagine ingrandita](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image9.png))


Dopo aver completato questa procedura guidata selezionare la casella di controllo Abilita modifica smart tag del controllo GridView.

Con il completamento della procedura guidata Crea origine dati da associare a GridView, Visual Studio ha creato la sintassi dichiarativa per entrambi i controlli. Passare alla visualizzazione origine per controllare dichiarativo di ObjectDataSource, riportata di seguito:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample2.aspx)]

Poiché nessun mapping per ObjectDataSource `Insert()` e `Delete()` metodi, esistono alcun `InsertParameters` o `DeleteParameters` sezioni. Inoltre, poiché il `Update()` metodo viene eseguito il mapping per il `UpdateProduct` overload del metodo che accetta solo tre parametri di input, il `UpdateParameters` sezione ha solo tre `Parameter` istanze.

Si noti che il ObjectDataSource `OldValuesParameterFormatString` è impostata su `original_{0}`. Questa proprietà viene impostata automaticamente da Visual Studio quando si utilizza la configurazione guidata origine dati. Tuttavia, poiché i metodi BLL non prevedono l'originale `ProductID` valore da passare, rimuovere completamente questa assegnazione di proprietà dalla sintassi dichiarativa di ObjectDataSource.

> [!NOTE]
> Se si deseleziona semplicemente di `OldValuesParameterFormatString` valore della proprietà dalla finestra delle proprietà nella visualizzazione progettazione, la proprietà continuano a esistere nella sintassi dichiarativa, ma verrà impostato su una stringa vuota. Rimuovere la proprietà completamente la sintassi dichiarativa o la finestra proprietà imposta il valore per l'impostazione predefinita, `{0}`.


Mentre ObjectDataSource ha solo `UpdateParameters` per nome del prodotto, prezzo e ID, Visual Studio ha aggiunto un BoundField o CheckBoxField in GridView per ognuno dei campi del prodotto.


[![GridView contiene un BoundField o CheckBoxField per ognuno dei campi del prodotto](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image11.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image10.png)

**Figura 4**: GridView contiene un BoundField o CheckBoxField per ognuno dei campi del prodotto ([fare clic per visualizzare l'immagine ingrandita](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image12.png))


Quando l'utente finale modifica un prodotto e fa clic sul pulsante di aggiornamento, GridView enumera i campi che non erano di sola lettura. Viene quindi impostato il valore del parametro corrispondente nella ObjectDataSource `UpdateParameters` insieme al valore specificato dall'utente. Se non è un parametro corrispondente, il controllo GridView aggiunge alla raccolta. Pertanto, se il controllo GridView contiene BoundField e CheckBoxFields per tutti i campi del prodotto, ObjectDataSource finirà richiamando il `UpdateProduct` overload che accetta in tutti questi parametri, nonostante il fatto che di ObjectDataSource markup dichiarativo specifica solo tre parametri di input (vedere Figura 5). Analogamente, se è presente una combinazione di non di sola lettura prodotto campi in GridView che non corrispondono ai parametri di input per un `UpdateProduct` overload, verrà generata un'eccezione durante il tentativo di aggiornamento.


[![Verrà eseguito il controllo GridView. aggiungere parametri alla raccolta UpdateParameters ObjectDataSource](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image14.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image13.png)

**Figura 5**: il GridView verranno aggiungere parametri di ObjectDataSource `UpdateParameters` raccolta ([fare clic per visualizzare l'immagine ingrandita](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image15.png))


Per garantire che ObjectDataSource richiama il `UpdateProduct` overload che accetta solo nome del prodotto, prezzo e ID, è necessario limitare il GridView con i campi modificabili per solo il `ProductName` e `UnitPrice`. Questo può essere ottenuto rimuovendo gli altri BoundField e CheckBoxFields, impostando gli altri campi `ReadOnly` proprietà `true`, o da una combinazione dei due. Per questa esercitazione verrà semplicemente rimuovere tutti i campi di GridView eccetto il `ProductName` e `UnitPrice` BoundField, dopo il quale markup dichiarativo del controllo GridView avrà un aspetto simile:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample3.aspx)]

Anche se il `UpdateProduct` overload prevede tre parametri di input, sono disponibili solo due BoundField nel nostro GridView. In questo modo il `productID` parametro di input è un valore di chiave primaria e passati tramite il valore della `DataKeyNames` proprietà per la riga modificata.

Il GridView, insieme al `UpdateProduct` overload, consente agli utenti di modificare solo il nome e il prezzo di un prodotto senza perdere gli altri campi di prodotto.


[![L'interfaccia consente la modifica solo il nome del prodotto e prezzo](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image17.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image16.png)

**Figura 6**: consente l'interfaccia modifica solo il nome del prodotto e prezzo ([fare clic per visualizzare l'immagine ingrandita](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image18.png))


> [!NOTE]
> Come descritto nell'esercitazione precedente, è estremamente importante che lo stato di visualizzazione s essere GridView abilitato (comportamento predefinito). Se si imposta la s GridView `EnableViewState` proprietà `false`, si corre il rischio di consentire agli utenti simultanei accidentalmente l'eliminazione o la modifica di record. Vedere [avviso: problema di concorrenza con ASP.NET 2.0 GridView/DetailsView/FormViews che supportano la modifica e/o l'eliminazione e il cui stato di visualizzazione è disabilitato](http://scottonwriting.net/sowblog/archive/2006/10/03/163215.aspx) per ulteriori informazioni.


## <a name="improving-theunitpriceformatting"></a>Migliorare la`UnitPrice`formattazione

L'esempio di GridView illustrato nella figura 6 works, mentre il `UnitPrice` campo non è formattato affatto, risultante in una visualizzazione di prezzo che non dispone di qualsiasi valuta i simboli e dispone di quattro cifre decimali. Per applicare una formattazione per le righe non modificabili della valuta, impostare semplicemente il `UnitPrice` del BoundField `DataFormatString` proprietà `{0:c}` e il relativo `HtmlEncode` proprietà `false`.


[![Impostare di conseguenza il prezzo unitario DataFormatString e proprietà HtmlEncode](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image20.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image19.png)

**Figura 7**: impostare il `UnitPrice`del `DataFormatString` e `HtmlEncode` proprietà conseguenza ([fare clic per visualizzare l'immagine ingrandita](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image21.png))


Questa modifica, le righe non modificabili di formattare il prezzo come valuta; Tuttavia, la riga modificata, Visualizza ancora il valore con quattro cifre decimali e senza il simbolo di valuta.


[![Le righe non modificabili vengono ora formattate come valori di valuta](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image23.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image22.png)

**Figura 8**: righe Non modificabili vengono ora formattate come valori di valuta ([fare clic per visualizzare l'immagine ingrandita](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image24.png))


Le istruzioni di formattazione specificate nel `DataFormatString` proprietà può essere applicata per l'interfaccia di modifica impostando il BoundField `ApplyFormatInEditMode` proprietà `true` (il valore predefinito è `false`). È opportuno impostare questa proprietà su `true`.


[![Impostare proprietà ApplyFormatInEditMode di UnitPrice BoundField su true](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image26.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image25.png)

**Figura 9**: impostare il `UnitPrice` del BoundField `ApplyFormatInEditMode` proprietà `true` ([fare clic per visualizzare l'immagine ingrandita](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image27.png))


Con questa modifica, il valore del `UnitPrice` nell'apportate riga anche viene formattata come valuta.


[![Il valore di UnitPrice della riga modificata è ora formattati come valuta](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image29.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image28.png)

**Figura 10**: la modifica della riga `UnitPrice` valore è ora formattati come valuta ([fare clic per visualizzare l'immagine ingrandita](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image30.png))


Tuttavia, l'aggiornamento di un prodotto con il simbolo di valuta nella casella di testo, ad esempio $19.00 genera un `FormatException`. Quando il controllo GridView tenta di assegnare i valori specificati dall'utente di ObjectDataSource `UpdateParameters` raccolta non è in grado di convertire il `UnitPrice` stringa "$19.00" il `decimal` richiesto dal parametro (vedere Figura 11). Per risolvere questo problema è possibile creare un gestore eventi per il controllo GridView `RowUpdating` evento e l'analisi fornita dall'utente `UnitPrice` come un formato valuta `decimal`.

Il GridView `RowUpdating` evento accetta come secondo parametro di un oggetto di tipo [GridViewUpdateEventArgs](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridviewupdateeventargs(VS.80).aspx), che include un `NewValues` dizionario come una delle relative proprietà che contiene i valori specificati dall'utente è pronti per essere assegnato a ObjectDataSource `UpdateParameters` insieme. È possibile sovrascrivere esistente `UnitPrice` valore il `NewValues` insieme con un valore decimale analizzato utilizzando il formato di valuta con le seguenti righe di codice nel `RowUpdating` gestore eventi:


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample4.cs)]

Se l'utente ha specificato un `UnitPrice` valore (ad esempio "$19.00"), questo valore viene sovrascritto con il valore decimale calcolato da [Decimal. Parse](https://msdn.microsoft.com/en-us/library/system.decimal.parse(VS.80).aspx), analisi del valore come valuta. Questa operazione verrà analizzare correttamente il separatore decimale in caso di tutti i simboli di valuta, virgole, decimali e così via e utilizza il [enumerazione NumberStyles](https://msdn.microsoft.com/en-US/library/system.globalization.numberstyles(VS.80).aspx) nel [System. Globalization](https://msdn.microsoft.com/en-US/library/abeh092z(VS.80).aspx) dello spazio dei nomi.

Figura 11 mostra sia il problema causato da simboli di valuta in fornito dall'utente `UnitPrice`, insieme a come il controllo GridView `RowUpdating` gestore dell'evento può essere utilizzato per analizzare correttamente l'input di questo tipo.


[![Il valore di UnitPrice della riga modificata è ora formattati come valuta](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image32.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image31.png)

**Figura 11**: la modifica della riga `UnitPrice` valore è ora formattati come valuta ([fare clic per visualizzare l'immagine ingrandita](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image33.png))


## <a name="step-2-prohibitingnull-unitprices"></a>Passaggio 2: proibizione delle`NULL UnitPrices`

Mentre il database è configurato per consentire `NULL` i valori di `Products` della tabella `UnitPrice` colonna, potrebbe essere necessario impedire agli utenti di visitare questa pagina specificare un `NULL` `UnitPrice` valore. Ovvero, se un utente non immette una `UnitPrice` valore quando si modifica una riga di prodotto, anziché salvare i risultati nel database che si desidera visualizzare un messaggio che informa l'utente che, tramite questa pagina, i prodotti modificati devono avere un prezzo specificato.

Il `GridViewUpdateEventArgs` oggetto passato a GridView `RowUpdating` gestore dell'evento contiene un `Cancel` proprietà che, se impostato su `true`, termina il processo di aggiornamento. È possibile estendere il `RowUpdating` gestore eventi per impostare `e.Cancel` per `true` e viene visualizzato un messaggio che spiega il motivo, se il `UnitPrice` valore il `NewValues` raccolta è `null`.

Per iniziare, aggiungere un controllo etichetta Web alla pagina denominata `MustProvideUnitPriceMessage`. Questo controllo etichetta verrà visualizzato se l'utente non riesce a specificare un `UnitPrice` valore durante l'aggiornamento di un prodotto. Impostare l'etichetta `Text` proprietà su "È necessario fornire un prezzo del prodotto". È stato creato anche una nuova classe CSS in `Styles.css` denominato `Warning` con la definizione seguente:


[!code-css[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample5.css)]

Infine, impostare l'etichetta `CssClass` proprietà `Warning`. A questo punto la finestra di progettazione deve visualizzare il messaggio di avviso in un colore rosso, grassetto, corsivo dimensione molto grande sopra GridView, come illustrato nella figura 12.


[![È stata aggiunta un'etichetta sopra il controllo GridView.](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image35.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image34.png)

**Figura 12**: un'etichetta è stato aggiunto di sopra GridView in cui viene ([fare clic per visualizzare l'immagine ingrandita](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image36.png))


Per impostazione predefinita, deve essere nascosto questa etichetta, quindi impostare il relativo `Visible` proprietà `false` nel `Page_Load` gestore eventi:


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample6.cs)]

Se l'utente tenta di aggiornare un prodotto senza specificare il `UnitPrice`, di voler annullare l'aggiornamento e visualizzare l'etichetta di avviso. Aumentare il controllo GridView `RowUpdating` gestore dell'evento come indicato di seguito:


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample7.cs)]

Se un utente tenta di salvare un prodotto senza specificare un prezzo, l'aggiornamento è stata annullata e viene visualizzato un messaggio utile. Mentre il database e logica di business, consente di `NULL` `UnitPrice` s, questa pagina ASP.NET non.


[![Un utente non può lasciare vuota UnitPrice](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image38.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image37.png)

**Figura 13**: un utente non può lasciare `UnitPrice` vuoto ([fare clic per visualizzare l'immagine ingrandita](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image39.png))


Finora è stato descritto come utilizzare il controllo GridView `RowUpdating` evento per modificare i valori di parametro assegnati a ObjectDataSource `UpdateParameters` insieme nonché come annullare l'aggiornamento di elaborare completamente. Questi concetti trasferiti ai controlli DetailsView e FormView e si applicano anche alle inserimento ed eliminazione.

Queste attività possono essere eseguite anche a livello di ObjectDataSource tramite i gestori eventi per il relativo `Inserting`, `Updating`, e `Deleting` eventi. Questi eventi attivano prima che venga richiamato il metodo associato dell'oggetto sottostante e forniscono una possibilità di ultima possibilità di modificare la raccolta di parametri di input o annullare l'operazione definitiva. I gestori eventi per questi tre eventi vengono passati a un oggetto di tipo [ObjectDataSourceMethodEventArgs](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.objectdatasourcemethodeventargs(VS.80).aspx) dotato di due proprietà interessanti:

- [Annulla](https://msdn.microsoft.com/en-US/library/system.componentmodel.canceleventargs.cancel(VS.80).aspx), che, se impostato su `true`, Annulla l'operazione in corso
- [Parametri di input](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.objectdatasourcemethodeventargs.inputparameters(VS.80).aspx), ovvero la raccolta di `InsertParameters`, `UpdateParameters`, o `DeleteParameters`, a seconda che il gestore dell'evento sia per il `Inserting`, `Updating`, o `Deleting` evento

Per illustrare l'utilizzo con i valori dei parametri a livello di ObjectDataSource, includere consente un controllo DetailsView nella nostra pagina che consente agli utenti di aggiungere un nuovo prodotto. Questo controllo DetailsView verrà utilizzato per fornire un'interfaccia per aggiungere rapidamente un nuovo prodotto per il database. Per mantenere un'interfaccia utente coerente quando aggiungere un nuovo prodotto si consente all'utente di immettere solo i valori per il `ProductName` e `UnitPrice` campi. Per impostazione predefinita, i valori che non sono specificati nell'interfaccia di inserimento di DetailsView verranno impostati una `NULL` valore del database. Tuttavia, è possibile usare il ObjectDataSource `Inserting` evento per inserire i valori predefiniti diversi, come si vedrà a breve.

## <a name="step-3-providing-an-interface-to-add-new-products"></a>Passaggio 3: Fornendo un'interfaccia per aggiungere nuovi prodotti

Trascinare un controllo DetailsView dalla casella degli strumenti nella finestra di progettazione sopra GridView, cancellare il contenuto relativo `Height` e `Width` le proprietà e i binding per ObjectDataSource già presente nella pagina. Verrà aggiunta una BoundField o CheckBoxField per ognuno dei campi del prodotto. Poiché si desidera utilizzare questo controllo DetailsView per aggiungere nuovi prodotti, è necessario selezionare l'opzione Abilita inserimento dallo smart tag; Tuttavia, non sono disponibili queste opzioni perché ObjectDataSource `Insert()` (metodo) non è stato eseguito il mapping a un metodo il `ProductsBLL` classe (tenere presente che è impostato il mapping (nessuno) quando si configura l'origine dati, vedere la figura 3).

Per configurare ObjectDataSource, selezionare il collegamento Configura origine dati dal suo smart tag, avviare la procedura guidata. La prima schermata consente di modificare l'oggetto sottostante che ObjectDataSource è associato a; lasciare configurata `ProductsBLL`. Nella schermata successiva sono elencati i mapping dai metodi di ObjectDataSource per l'oggetto sottostante. Anche se è indicato in modo esplicito che il `Insert()` e `Delete()` metodi non devono essere mappati ai metodi, se si passa alle schede delle INSERT e DELETE si noterà che il mapping sia presente. In questo modo il `ProductsBLL`del `AddProduct` e `DeleteProduct` metodi utilizzano il `DataObjectMethodAttribute` attributo per indicare che sono i metodi predefiniti per `Insert()` e `Delete()`rispettivamente. Di conseguenza, la procedura guidata ObjectDataSource seleziona questi ogni volta che si esegue la procedura guidata a meno che non vi è un altro valore specificato in modo esplicito.

Lasciare il `Insert()` che punta al metodo di `AddProduct` (metodo), ma impostare nuovamente l'elenco di riepilogo a discesa della scheda DELETE su (nessuno).


[![Impostare l'elenco di riepilogo a discesa della scheda Inserisci il metodo AddProduct](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image41.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image40.png)

**Nella figura 14**: impostare l'elenco di riepilogo a discesa della scheda Inserisci di `AddProduct` metodo ([fare clic per visualizzare l'immagine ingrandita](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image42.png))


[![Impostare l'elenco di riepilogo a discesa della scheda DELETE su (nessuno)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image44.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image43.png)

**Figura 15**: impostare l'elenco di riepilogo a discesa della scheda eliminare su (nessuno) ([fare clic per visualizzare l'immagine ingrandita](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image45.png))


Dopo aver apportato queste modifiche, la sintassi dichiarativa di ObjectDataSource verrà espanso per includere un `InsertParameters` insieme, come illustrato di seguito:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample8.aspx)]

Ripetere la procedura guidata aggiunta nuovamente il `OldValuesParameterFormatString` proprietà. È opportuno cancellare questa proprietà impostandola sul valore predefinito (`{0}`) o rimuoverlo completamente dalla sintassi dichiarativa.

Con ObjectDataSource fornendo funzionalità di inserimento, smart tag di DetailsView ora includere la casella di controllo Abilita inserimento; tornare alla finestra di progettazione e selezionare questa opzione. Ridurre quindi il controllo DetailsView pertanto ha solo due BoundField - `ProductName` e `UnitPrice` - e di CommandField. La sintassi dichiarativa del controllo DetailsView a questo punto dovrebbe essere simile:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample9.aspx)]

La figura 16 Mostra questa pagina quando viene visualizzato tramite un browser a questo punto. Come si può notare, il controllo DetailsView Elenca il nome e il prezzo del prodotto prima (Chai). Ciò che si vuole, tuttavia, è un'interfaccia di inserimento che consente all'utente di aggiungere rapidamente un nuovo prodotto per il database.


[![Il controllo DetailsView è attualmente eseguito il rendering in modalità di sola lettura](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image47.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image46.png)

**Figura 16**: il controllo DetailsView viene attualmente eseguito il rendering in modalità di sola lettura ([fare clic per visualizzare l'immagine ingrandita](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image48.png))


Per visualizzare il controllo DetailsView nella modalità di inserimento è necessario impostare il `DefaultMode` proprietà `Inserting`. In questo viene eseguito il rendering di DetailsView in modalità di inserimento alla prima visita e, dopo aver inserito un nuovo record. Come mostrato nella figura 17, tale controllo DetailsView fornisce un'interfaccia rapida per l'aggiunta di un nuovo record.


[![DetailsView fornisce un'interfaccia per aggiungere rapidamente un nuovo prodotto](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image50.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image49.png)

**Figura 17**: DetailsView fornisce un'interfaccia per aggiungere rapidamente un nuovo prodotto ([fare clic per visualizzare l'immagine ingrandita](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image51.png))


Quando l'utente immette un nome di prodotto e prezzo (ad esempio "Acqua Acme" e 1.99, come illustrato nella figura 17) e fa clic su Inserisci, viene utilizzata un postback e avvia il flusso di lavoro di inserimento, che si conclude con un nuovo record di prodotto viene aggiunto al database. DetailsView gestisce l'interfaccia di inserimento e controllo GridView viene automaticamente riassociati all'origine dati per includere il nuovo prodotto, come illustrato nella figura 18.


![Il prodotto](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image52.png)

**Figura 18**: il prodotto "Acqua Acme" è stato aggiunto al Database


Mentre GridView della figura 18, non è evidente, i campi prodotto mancante dall'interfaccia di DetailsView `CategoryID`, `SupplierID`, `QuantityPerUnit`e così via vengono assegnati `NULL` i valori del database. È possibile vedere questo eseguendo i passaggi seguenti:

1. Accedere a Esplora Server in Visual Studio
2. Espansione di `NORTHWND.MDF` nodo del database
3. Fare clic su di `Products` nodo della tabella di database
4. Selezionare Mostra dati tabella

Questo modo vengono elencati tutti i record di `Products` tabella. Come nella figura 19, tutte le colonne del prodotto con questo nuovo diverso da `ProductID`, `ProductName`, e `UnitPrice` hanno `NULL` valori.


[![I campi il prodotto non specificato nel controllo DetailsView vengono assegnati i valori NULL](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image54.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image53.png)

**Figura 19**: il prodotto campi non specificato nel controllo DetailsView assegnati `NULL` valori ([fare clic per visualizzare l'immagine ingrandita](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image55.png))


Potrebbe essere necessario specificare un valore predefinito diverso da `NULL` per uno o più di questi valori di colonna, sia perché `NULL` non è la migliore opzione predefinita o perché la colonna del database non consente `NULL` s. A tale scopo è possibile impostare a livello di codice i valori dei parametri di DetailsView `InputParameters` insieme. Questa assegnazione può essere eseguita sia nell'evento gestore per il DetailsView `ItemInserting` evento o di ObjectDataSource `Inserting` evento. Poiché descritti in precedenza usando gli eventi di pre e post-livelli dati Web controllare livello, esaminiamo utilizzando gli eventi di ObjectDataSource questo momento.

## <a name="step-4-assigning-values-to-thecategoryidandsupplieridparameters"></a>Passaggio 4: Assegnazione di valori per il`CategoryID`e`SupplierID`parametri

Per questa esercitazione si supponga che per l'applicazione quando si aggiunge un nuovo prodotto tramite questa interfaccia deve essere assegnato un `CategoryID` e `SupplierID` valore pari a 1. Come accennato in precedenza, ObjectDataSource ha una coppia di eventi di pre e post-livelli di generazione durante il processo di modifica dei dati. Quando il relativo `Insert()` metodo viene richiamato, ObjectDataSource genera prima relativo `Inserting` evento, quindi chiama il metodo che relativo `Insert()` è stato mappato al metodo e infine genera il `Inserted` evento. Il `Inserting` gestore mette a disposizione ci un'ultima opportunità per modificare i parametri di input o annullare l'operazione è definitiva.

> [!NOTE]
> In un'applicazione reale si desidera probabilmente in modo che l'utente, specificare la categoria e il fornitore o si seleziona questo valore per in base ad alcuni criteri o business logica (anziché a selezionare un ID 1). Indipendentemente dal fatto che, nell'esempio viene illustrato come impostare a livello di codice il valore di un parametro di input da un evento di pre-livello di ObjectDataSource.


È opportuno creare un gestore eventi per ObjectDataSource `Inserting` evento. Si noti che secondo parametro del gestore dell'evento è un oggetto di tipo `ObjectDataSourceMethodEventArgs`, che dispone di una proprietà per accedere alla raccolta di parametri (`InputParameters`) e una proprietà per annullare l'operazione (`Cancel`).


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample10.cs)]

A questo punto, il `InputParameters` proprietà contiene il ObjectDataSource `InsertParameters` insieme con i valori assegnati dal controllo DetailsView. Per modificare il valore di uno di questi parametri, utilizzare semplicemente: `e.InputParameters["paramName"] = value`. Pertanto, per impostare il `CategoryID` e `SupplierID` valori pari a 1, regolare il `Inserting` gestore simile al seguente:


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample11.cs)]

Questo tempo quando si aggiunge un nuovo prodotto (ad esempio Acme Soda), il `CategoryID` e `SupplierID` colonne del nuovo prodotto vengono impostate su 1 (vedere Figura 20).


[![Nuovi prodotti presentano ora CategoryID e SupplierID valori impostati su 1](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image57.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image56.png)

**Figura 20**: nuovi prodotti ora sono relativi `CategoryID` e `SupplierID` valori impostati su 1 ([fare clic per visualizzare l'immagine ingrandita](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image58.png))


## <a name="summary"></a>Riepilogo

Durante la modifica, inserimento ed eliminazione di processo, sia i dati Web e il controllo ObjectDataSource procedere con un numero di eventi di pre e post-livelli. In questa esercitazione è esaminare gli eventi di pre-livelli e stato illustrato come utilizzare questi per personalizzare i parametri di input o annullare l'operazione di modifica di dati sia del tutto dal controllo Web dati eventi ObjectDataSource. Nella prossima esercitazione verrà esaminato la creazione e utilizzo dei gestori eventi per gli eventi post-livelli.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Lead revisori per questa esercitazione sono stati Jackie Goor e Liz Shulok. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Precedente](an-overview-of-inserting-updating-and-deleting-data-cs.md)
[Successivo](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md)
