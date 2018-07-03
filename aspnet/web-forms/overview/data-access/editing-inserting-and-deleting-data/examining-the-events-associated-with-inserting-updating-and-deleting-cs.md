---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs
title: Analisi degli eventi associati a inserimento, aggiornamento ed eliminazione (c#) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione che verrà esaminato usando gli eventi che si verificano prima, durante e dopo un'operazione di inserimento, aggiornamento o di eliminazione di un controllo Web di dati ASP.NET. W....
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: dab291a0-a8b5-46fa-9dd8-3d35b249201f
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs
msc.type: authoredcontent
ms.openlocfilehash: cec25809038cbe6a114b0e36ef7265d84bbe9b92
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363300"
---
<a name="examining-the-events-associated-with-inserting-updating-and-deleting-c"></a>Esaminare gli eventi associati a inserimento, aggiornamento ed eliminazione (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'App di esempio](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_17_CS.exe) o [Scarica il PDF](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/datatutorial17cs1.pdf)

> In questa esercitazione che verrà esaminato usando gli eventi che si verificano prima, durante e dopo un'operazione di inserimento, aggiornamento o di eliminazione di un controllo Web di dati ASP.NET. Si vedrà anche come personalizzare l'interfaccia di modifica per aggiornare solo un subset dei campi del prodotto.


## <a name="introduction"></a>Introduzione

Quando si usa l'inserimento incorporato, modifica o eliminazione di funzionalità dei controlli GridView, DetailsView e FormView, dovrà attendere una serie di passaggi quando l'utente finale completa il processo di aggiunta di un nuovo record o l'aggiornamento o eliminazione di un record esistente. Come accennato nel [esercitazione precedente](an-overview-of-inserting-updating-and-deleting-data-cs.md), quando una riga viene modificata in GridView sul pulsante di modifica viene sostituito dall'aggiornamento e annullamento pulsanti e l'attivazione BoundField nelle caselle di testo. Dopo che l'utente finale Aggiorna i dati e fa clic su Aggiorna, durante il postback vengono eseguiti i passaggi seguenti:

1. GridView popola ObjectDataSource `UpdateParameters` con i campi di identificazione univoco del record modificati (tramite il `DataKeyNames` proprietà) con i valori immessi dall'utente
2. Il controllo GridView richiama ObjectDataSource `Update()` metodo, che a sua volta richiama il metodo appropriato nell'oggetto sottostante (`ProductsDAL.UpdateProduct`, nell'esercitazione precedente)
3. I dati sottostanti, che includono ora le modifiche aggiornate, sono riassociati per il controllo GridView

Durante questa sequenza di passaggi, generare un numero di eventi, ci permette di creare gestori eventi per aggiungere una logica personalizzata dove necessario. Ad esempio, prima di passaggio 1, il controllo GridView `RowUpdating` viene generato l'evento. È a questo punto, possiamo, annullare la richiesta di aggiornamento nel caso di errori di convalida. Quando la `Update()` metodo viene richiamato, di ObjectDataSource `Updating` attivazione dell'evento fornisce la possibilità di aggiungere o personalizzare i valori di uno qualsiasi del `UpdateParameters`. Dopo aver sottostante di ObjectDataSource metodo dell'oggetto è stata completata l'esecuzione, di ObjectDataSource `Updated` viene generato l'evento. Un gestore eventi per il `Updated` eventi possono esaminare i dettagli sull'operazione di aggiornamento, ad esempio il numero di righe interessato e se si è verificata un'eccezione. Infine, dopo il di passaggio 2, il controllo GridView `RowUpdated` un gestore eventi per questo evento può esaminare le informazioni aggiuntive sull'operazione di aggiornamento appena eseguite, viene generato l'evento.

Figura 1 viene illustrata questa serie di eventi e i passaggi durante l'aggiornamento di un controllo GridView. Lo schema di eventi nella figura 1 non è univoco per l'aggiornamento con un controllo GridView. Inserimento, aggiornamento o eliminazione di dati da GridView, DetailsView e FormView precipitates la stessa sequenza di eventi di pre-elaborazione e post-livelli per il controllo Web per dati e di ObjectDataSource.


[![Una serie di pre- e attivare gli post-eventi di quando si aggiornano i dati in un controllo GridView](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image2.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image1.png)

**Figura 1**: una serie di pre-elaborazione e gli post-eventi di Fire quando l'aggiornamento dati in un controllo GridView ([fare clic per visualizzare l'immagine con dimensioni normali](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image3.png))


In questa esercitazione che verranno presi in esame usando questi eventi per estendere l'inserimento di predefinite, aggiornamento ed eliminazione di funzionalità dei dati ASP.NET Web controlla. Si vedrà anche come personalizzare l'interfaccia di modifica per aggiornare solo un subset dei campi del prodotto.

## <a name="step-1-updating-a-productsproductnameandunitpricefields"></a>Passaggio 1: Aggiornare un prodotto`ProductName`e`UnitPrice`campi

In modifica interfacce dall'esercitazione precedente *tutti* campi prodotto che non erano sola lettura devono essere incluse. Se dovessimo per rimuovere un campo da GridView - pronunciare `QuantityPerUnit` : quando l'aggiornamento dei dati di controllo Web per dati non imposterebbe di ObjectDataSource `QuantityPerUnit` `UpdateParameters` valore. Quindi passa l'oggetto ObjectDataSource in un `null` valore il `UpdateProduct` metodo livello per la logica di Business (BLL), che potrebbe modificare il record di database modificato `QuantityPerUnit` colonna da un `NULL` valore. Analogamente, se un campo obbligatorio, ad esempio `ProductName`, viene rimosso dall'interfaccia di modifica, l'aggiornamento avrà esito negativo con un "*colonna"NomeProdotto"non consente valori null*" eccezione. Il motivo di questo comportamento è stato perché ObjectDataSource è stato configurato per chiamare il `ProductsBLL` della classe `UpdateProduct` (metodo), che prevede un parametro di input per ognuno dei campi di prodotto. Pertanto, di ObjectDataSource `UpdateParameters` raccolta contiene un parametro per ogni metodo di input parametri.

Se si desidera fornire un controllo Web che consente all'utente finale aggiornare solo un sottoinsieme dei campi di dati, quindi, dobbiamo impostare a livello di codice il mancante `UpdateParameters` valori di ObjectDataSource `Updating` gestore dell'evento o creare e chiamare un metodo BLL che è previsto solo un subset dei campi. Esaminiamo questo secondo approccio.

In particolare, è possibile creare una pagina che visualizza solo il `ProductName` e `UnitPrice` campi in un controllo GridView modificabile. Interfaccia per la modifica di questo GridView consentirà solo all'utente di aggiornare i campi visualizzati due `ProductName` e `UnitPrice`. Poiché questa interfaccia per la modifica fornisce solo un sottoinsieme dei campi di un prodotto, è necessario creare un oggetto ObjectDataSource che usa il livello BLL esistenti `UpdateProduct` metodo e ha i valori dei campi mancanti prodotto impostato a livello di codice nella relativa `Updating` evento gestore oppure è necessario creare un nuovo metodo BLL che prevede solo il subset dei campi definiti in GridView. Per questa esercitazione, è possibile usare l'opzione di quest'ultima e creare un overload del `UpdateProduct` metodo, uno che accetta solo tre parametri di input: `productName`, `unitPrice`, e `productID`:


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample1.cs)]

Ad esempio originale `UpdateProduct` metodo, questo overload inizia da un controllo per verificare se nel database con l'oggetto specificato è un prodotto `ProductID`. Se non viene restituito `false`, che indica che la richiesta per aggiornare le informazioni sul prodotto non è riuscita. In caso contrario, aggiorna il record di prodotto esistente `ProductName` e `UnitPrice` campi conseguenza ed esegue il commit dell'aggiornamento chiamando il TableAdpater `Update()` metodo, passando il `ProductsRow` istanza.

Con questa aggiunta al nostro `ProductsBLL` (classe), siamo pronti per creare l'interfaccia di GridView semplificata. Aprire il `DataModificationEvents.aspx` nella `EditInsertDelete` cartella e aggiungere un controllo GridView alla pagina. Creare un nuovo oggetto ObjectDataSource e configurarlo per usare il `ProductsBLL` classe con relativo `Select()` mapping del metodo per `GetProducts` e la relativa `Update()` mapping del metodo per il `UpdateProduct` overload che accetta solo il `productName`, `unitPrice`, e `productID` parametri di input. Figura 2 illustra la procedura guidata Creazione di un'origine dati durante il mapping di ObjectDataSource `Update()` metodo per il `ProductsBLL` della nuova classe `UpdateProduct` overload del metodo.


[![Map (metodo) di ObjectDataSource Update () per il nuovo Overload UpdateProduct](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image5.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image4.png)

**Figura 2**: eseguire il mapping di ObjectDataSource `Update()` per il nuovo metodo `UpdateProduct` Overload ([fare clic per visualizzare l'immagine con dimensioni normali](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image6.png))


Poiché questo esempio sarà necessario inizialmente solo la possibilità di modificare i dati, ma non inserire o eliminare i record, è opportuno indicare esplicitamente che ObjectDataSource `Insert()` e `Delete()` metodi non devono essere mappati a uno qualsiasi del `ProductsBLL` metodi della classe accedendo alle schede INSERT e DELETE e (nessuno) scegliendo dall'elenco a discesa.


[![Scegliere (nessuno) nell'elenco a discesa per le schede DELETE e INSERT](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image8.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image7.png)

**Figura 3**: scegliere (nessuno) da the List elenco a discesa per l'inserimento ed eliminare schede ([fare clic per visualizzare l'immagine con dimensioni normali](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image9.png))


Dopo aver completato questa procedura guidata selezionare la casella di controllo Abilita modifica dallo smart tag del controllo GridView.

Con il completamento della procedura guidata Creazione di un'origine dati da associare a GridView, Visual Studio ha creato la sintassi dichiarativa per entrambi i controlli. Passare alla visualizzazione origine per esaminare markup dichiarativo di ObjectDataSource, mostrata di seguito:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample2.aspx)]

Poiché non sono disponibili mapping per ObjectDataSource `Insert()` e `Delete()` metodi, esistono nessun `InsertParameters` o `DeleteParameters` sezioni. Inoltre, poiché il `Update()` metodo viene eseguito il mapping al `UpdateProduct` overload del metodo che accetta solo tre parametri di input, il `UpdateParameters` sezione ha solo tre `Parameter` istanze.

Si noti che ObjectDataSource `OldValuesParameterFormatString` è impostata su `original_{0}`. Questa proprietà viene impostata automaticamente da Visual Studio quando si usa la procedura guidata Configura origine dati. Tuttavia, poiché i metodi BLL inaspettatamente originale `ProductID` valore da passare, rimuovere completamente questa assegnazione di proprietà da sintassi dichiarativa di ObjectDataSource.

> [!NOTE]
> Se sufficiente cancellare i `OldValuesParameterFormatString` rimarranno nella sintassi dichiarativa per il valore della proprietà dalla finestra delle proprietà nella visualizzazione progettazione, la proprietà, ma verrà impostato su una stringa vuota. Rimuovere la proprietà completamente dalla sintassi dichiarativa o, dalla finestra delle proprietà, imposta il valore per l'impostazione predefinita, `{0}`.


Anche se dispone solo di ObjectDataSource `UpdateParameters` per nome del prodotto, prezzo e ID, Visual Studio ha aggiunto un CampoCasellaDiControllo o un BoundField in GridView per ognuno dei campi del prodotto.


[![Il controllo GridView contiene un BoundField o CampoCasellaDiControllo per ognuno dei campi del prodotto](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image11.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image10.png)

**Figura 4**: il controllo GridView contiene un BoundField o CampoCasellaDiControllo per ognuno dei campi del prodotto ([fare clic per visualizzare l'immagine con dimensioni normali](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image12.png))


Quando l'utente finale modifica un prodotto e fa clic sul relativo pulsante di aggiornamento, il controllo GridView enumera i campi che non erano di sola lettura. Viene quindi impostato il valore del parametro corrispondente di ObjectDataSource `UpdateParameters` insieme al valore immesso dall'utente. Se non è presente un parametro corrispondente, il controllo GridView aggiunge alla raccolta. Pertanto, se il controllo GridView contiene BoundField e CheckBoxFields per tutti i campi del prodotto, ObjectDataSource finirà richiamando il `UpdateProduct` overload che accetta tutti questi parametri, nonostante il fatto che di ObjectDataSource markup dichiarativo specifica solo tre parametri di input (vedere la figura 5). Analogamente, se è presente una combinazione di non di sola lettura prodotti campi in GridView che non corrispondono ai parametri di input per un `UpdateProduct` esegue l'overload, verrà generata un'eccezione durante il tentativo di aggiornare.


[![La classe GridView aggiungere parametri UpdateParameters insieme di ObjectDataSource](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image14.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image13.png)

**Figura 5**: il controllo GridView verrà aggiungere parametri di ObjectDataSource `UpdateParameters` raccolta ([fare clic per visualizzare l'immagine con dimensioni normali](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image15.png))


Per garantire che ObjectDataSource richiama il `UpdateProduct` overload che accetta solo nome del prodotto, prezzo e ID, è necessario limitare il controllo GridView alla pressione dei campi modificabili per il `ProductName` e `UnitPrice`. A questo scopo tramite la rimozione di altri BoundField e CheckBoxFields, impostando quelli degli altri campi `ReadOnly` proprietà `true`, o da una combinazione dei due. Per questa esercitazione è sufficiente rimuovere tutti i campi di GridView, ad eccezione di `ProductName` e `UnitPrice` BoundField, dopo il quale markup dichiarativo del controllo GridView avrà un aspetto simile:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample3.aspx)]

Anche se il `UpdateProduct` overload prevede tre parametri di input, abbiamo solo due BoundField nel nostro GridView. Infatti, il `productID` parametro di input è un valore di chiave primaria e passati tramite il valore della `DataKeyNames` proprietà per la riga modificata.

Il controllo GridView, insieme al `UpdateProduct` esegue l'overload, consente agli utenti di modificare solo il nome e il prezzo di un prodotto senza perdere eventuali altri campi del prodotto.


[![L'interfaccia consente la modifica solo il nome del prodotto e prezzo](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image17.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image16.png)

**Figura 6**: consente l'interfaccia modifica semplicemente il nome del prodotto e prezzo ([fare clic per visualizzare l'immagine con dimensioni normali](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image18.png))


> [!NOTE]
> Come illustrato nell'esercitazione precedente, è estremamente importante che il controllo GridView lo stato di visualizzazione s essere abilitato (comportamento predefinito). Se si imposta la s GridView `EnableViewState` proprietà `false`, si corre il rischio di consentire agli utenti simultanei involontariamente l'eliminazione o modifica di record. Visualizzare [avviso: problema di concorrenza con ASP.NET 2.0 GridViews/DetailsView/FormViews tale supporto di modifica e/o l'eliminazione e il cui stato di visualizzazione è disabilitato](http://scottonwriting.net/sowblog/archive/2006/10/03/163215.aspx) per altre informazioni.


## <a name="improving-theunitpriceformatting"></a>Miglioramento di`UnitPrice`formattazione

L'esempio di GridView illustrato nella figura 6 works, mentre il `UnitPrice` campo non è formattato affatto, risultante in una visualizzazione di prezzo che non dispone di qualsiasi tipo di valuta i simboli e dispone di quattro cifre decimali. Per applicare una formattazione per le righe non modificabili della valuta, impostare semplicemente la `UnitPrice` del BoundField `DataFormatString` proprietà `{0:c}` e la relativa `HtmlEncode` proprietà `false`.


[![Impostare di conseguenza l'elemento UnitPrice DataFormatString e proprietà HtmlEncode](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image20.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image19.png)

**Figura 7**: impostare il `UnitPrice`del `DataFormatString` e `HtmlEncode` le proprietà di conseguenza ([fare clic per visualizzare l'immagine con dimensioni normali](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image21.png))


Con questa modifica, le righe non modificabili formattare il prezzo come una valuta; la riga modificata, tuttavia, ancora visualizzato il valore con quattro cifre decimali e senza il simbolo di valuta.


[![Righe non modificabili vengono ora formattate come valori di valuta](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image23.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image22.png)

**Figura 8**: le righe Non modificabili vengono ora formattate come valori di valuta ([fare clic per visualizzare l'immagine con dimensioni normali](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image24.png))


Istruzioni di formattazione specificate nella `DataFormatString` proprietà può essere applicata all'interfaccia di modifica impostando il BoundField `ApplyFormatInEditMode` proprietà `true` (il valore predefinito è `false`). Si consiglia di impostare questa proprietà su `true`.


[![Proprietà ApplyFormatInEditMode di UnitPrice BoundField impostata su true](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image26.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image25.png)

**Figura 9**: impostare il `UnitPrice` del BoundField `ApplyFormatInEditMode` proprietà `true` ([fare clic per visualizzare l'immagine con dimensioni normali](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image27.png))


Con questa modifica, il valore del `UnitPrice` zobrazené modificata la riga viene anche formattata come una valuta.


[![Valore di UnitPrice della riga modificata viene ora formattate come valuta](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image29.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image28.png)

**Figura 10**: la modifica della riga `UnitPrice` valore viene formattato come una valuta ([fare clic per visualizzare l'immagine con dimensioni normali](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image30.png))


Tuttavia, l'aggiornamento di un prodotto con il simbolo di valuta nella casella di testo, ad esempio $19.00 genera un `FormatException`. Il controllo GridView quando tenta di assegnare i valori specificati dall'utente a ObjectDataSource `UpdateParameters` raccolta non è in grado di convertire il `UnitPrice` stringa "$19.00" nel `decimal` richiesto dal parametro (vedere la figura 11). Per risolvere il problema è possibile creare un gestore eventi per il controllo GridView `RowUpdating` evento e fare in modo che analizza il fornito dall'utente `UnitPrice` come un formato valuta `decimal`.

Il controllo GridView `RowUpdating` eventi accettano come secondo parametro un oggetto di tipo [GridViewUpdateEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewupdateeventargs(VS.80).aspx), che include un `NewValues` dizionario come una delle relative proprietà che contiene i valori specificati dall'utente è pronti per essere assegnato a ObjectDataSource `UpdateParameters` raccolta. È possibile sovrascrivere esistente `UnitPrice` valore il `NewValues` insieme con un valore decimale analizzata usando il formato di valuta con le seguenti righe di codice nel `RowUpdating` gestore dell'evento:


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample4.cs)]

Se l'utente ha specificato un `UnitPrice` valore (ad esempio "$19.00"), questo valore viene sovrascritto con il valore decimale calcolato dal [Decimal. Parse](https://msdn.microsoft.com/library/system.decimal.parse(VS.80).aspx), l'analisi del valore come valuta. Questa impostazione verrà correttamente analizzare il separatore decimale in caso di qualsiasi simbolo di valuta, virgole, decimali e così via e Usa il [enumerazione NumberStyles](https://msdn.microsoft.com/library/system.globalization.numberstyles(VS.80).aspx) nel [System. Globalization](https://msdn.microsoft.com/library/abeh092z(VS.80).aspx) dello spazio dei nomi.

Figura 11 mostra sia il problema causato dai simboli di valuta nel fornito dall'utente `UnitPrice`, nonché come GridView `RowUpdating` gestore eventi può essere utilizzato per analizzare correttamente tali input.


[![Valore di UnitPrice della riga modificata viene ora formattate come valuta](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image32.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image31.png)

**Figura 11**: la modifica della riga `UnitPrice` valore viene formattato come una valuta ([fare clic per visualizzare l'immagine con dimensioni normali](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image33.png))


## <a name="step-2-prohibitingnull-unitprices"></a>Passaggio 2: divieto di`NULL UnitPrices`

Mentre il database è configurato per consentire `NULL` i valori di `Products` della tabella `UnitPrice` colonna, si può decidere di evitare che gli utenti che visitano questa pagina determinata dall'impostazione di un `NULL` `UnitPrice` valore. Vale a dire, se un utente non immette una `UnitPrice` valore durante la modifica di una riga di prodotto, anziché salvare i risultati al database di cui si desidera visualizzare un messaggio che informa l'utente che, tramite questa pagina, nessuno dei prodotti modificata deve avere un prezzo specificato.

Il `GridViewUpdateEventArgs` oggetto passato in del controllo GridView `RowUpdating` gestore dell'evento contiene un `Cancel` proprietà che, se impostato su `true`, termina il processo di aggiornamento. È possibile estendere il `RowUpdating` gestore dell'evento per impostare `e.Cancel` al `true` e visualizzare un messaggio spiegherà il motivo per cui se il `UnitPrice` valore nel `NewValues` raccolta è `null`.

Iniziare aggiungendo un controllo etichetta Web alla pagina denominata `MustProvideUnitPriceMessage`. Questo controllo etichetta verrà visualizzato se l'utente non riesce a specificare un `UnitPrice` valore durante l'aggiornamento di un prodotto. Impostare l'etichetta `Text` proprietà su "È necessario fornire un prezzo per il prodotto". Ho creato anche una nuova classe CSS `Styles.css` denominato `Warning` con la definizione seguente:


[!code-css[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample5.css)]

Infine, impostare l'etichetta `CssClass` proprietà `Warning`. A questo punto la finestra di progettazione dovrebbe visualizzare il messaggio di avviso in un colore rosso, grassetto, corsivo, dimensione molto grande sopra il controllo GridView, come illustrato nella figura 12.


[![È stata aggiunta un'etichetta sopra il controllo GridView](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image35.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image34.png)

**Figura 12**: oggetto etichetta è stato aggiunto di sopra GridView in ([fare clic per visualizzare l'immagine con dimensioni normali](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image36.png))


Per impostazione predefinita, questa etichetta deve essere nascosta in modo da impostare relativi `Visible` proprietà `false` nel `Page_Load` gestore dell'evento:


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample6.cs)]

Se l'utente tenta di aggiornare un prodotto senza specificare il `UnitPrice`, si vuole annullare l'aggiornamento e visualizzare l'etichetta di avviso. Aumentare il controllo GridView `RowUpdating` gestore dell'evento come indicato di seguito:


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample7.cs)]

Se un utente tenta di salvare un prodotto senza specificare un prezzo, l'aggiornamento è stata annullata e viene visualizzato un messaggio utile. Mentre il database (e la logica di business) consente `NULL` `UnitPrice` s, non presenta questo particolare pagina ASP.NET.


[![Un utente non è possibile lasciare vuoto UnitPrice](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image38.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image37.png)

**Figura 13**: un utente non può lasciare `UnitPrice` vuoto ([fare clic per visualizzare l'immagine con dimensioni normali](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image39.png))


Finora abbiamo visto come utilizzare il controllo GridView `RowUpdating` eventi a livello di codice modificare i valori di parametro assegnati a ObjectDataSource `UpdateParameters` raccolta anche come annullare l'aggiornamento di elaborare completamente. Questi concetti si applicano ai controlli DetailsView e FormView e si applicano anche a inserimento ed eliminazione.

Queste attività possono essere eseguite anche a livello di ObjectDataSource mediante gestori eventi per i relativi `Inserting`, `Updating`, e `Deleting` eventi. Questi eventi vengono attivati prima che venga richiamato il metodo associato dell'oggetto sottostante e forniscono un'ultima opportunità per modificare la raccolta di parametri di input o annullare l'operazione definitiva. I gestori eventi per questi tre eventi vengono passati a un oggetto di tipo [ObjectDataSourceMethodEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourcemethodeventargs(VS.80).aspx) che dispone di due proprietà di interesse:

- [Annullare](https://msdn.microsoft.com/library/system.componentmodel.canceleventargs.cancel(VS.80).aspx)che, se impostato su `true`, Annulla l'operazione in corso
- [InputParameters](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourcemethodeventargs.inputparameters(VS.80).aspx), che è la raccolta di `InsertParameters`, `UpdateParameters`, o `DeleteParameters`, a seconda che il gestore dell'evento sia per il `Inserting`, `Updating`, o `Deleting` evento

Per illustrare l'utilizzo con i valori dei parametri a livello di ObjectDataSource, è possibile includere un controllo DetailsView nella nostra pagina che consente agli utenti di aggiungere un nuovo prodotto. Questo controllo DetailsView verrà utilizzato per fornire un'interfaccia per aggiungere rapidamente un nuovo prodotto al database. Da un'interfaccia utente coerente quando si aggiunge un nuovo prodotto è possibile consentire all'utente di immettere solo valori per il `ProductName` e `UnitPrice` campi. Per impostazione predefinita, i valori che non sono forniti nell'interfaccia di inserimento di DetailsView verranno impostati un `NULL` valore del database. Tuttavia, è possibile usare ObjectDataSource `Inserting` eventi per inserire i valori predefiniti diversi, come vedremo tra breve.

## <a name="step-3-providing-an-interface-to-add-new-products"></a>Passaggio 3: Fornire un'interfaccia per aggiungere nuovi prodotti

Trascinare un controllo DetailsView dalla casella degli strumenti nella finestra di progettazione sopra il controllo GridView, cancellare il contenuto relativo `Height` e `Width` proprietà e associare a ObjectDataSource già presente nella pagina. Si aggiungerà un BoundField o CampoCasellaDiControllo per ognuno dei campi del prodotto. Poiché si vuole usare questo controllo DetailsView per aggiungere nuovi prodotti, è necessario selezionare l'opzione Attiva paging nello smart tag; Tuttavia, non sono disponibili queste opzioni perché di ObjectDataSource `Insert()` metodo non viene eseguito il mapping a un metodo nel `ProductsBLL` classe (tenere presente che abbiamo impostato il mapping su (nessuno) quando l'origine dati di configurazione, vedere la figura 3).

Per configurare ObjectDataSource, selezionare il collegamento Configura origine dati dal suo smart tag, avviare la procedura guidata. La prima schermata consente di modificare l'oggetto sottostante che è associato l'oggetto ObjectDataSource; non modificare l'impostazione per `ProductsBLL`. Nella schermata successiva sono elencati i mapping dai metodi di ObjectDataSource dell'oggetto sottostante. Anche se è indicato in modo esplicito che il `Insert()` e `Delete()` metodi non devono essere associati a qualsiasi metodo, se si passa alle schede INSERT e DELETE si noterà che il mapping sia presente. Infatti il `ProductsBLL`del `AddProduct` e `DeleteProduct` metodi usano il `DataObjectMethodAttribute` attributo per indicare che sono i metodi predefiniti per `Insert()` e `Delete()`, rispettivamente. Di conseguenza, la procedura guidata ObjectDataSource consente di selezionare queste ogni volta che si esegue la procedura guidata a meno che non vi è un altro valore specificato in modo esplicito.

Lasciare il `Insert()` che punta al metodo il `AddProduct` (metodo), ma impostare nuovamente l'elenco di elenco a discesa della scheda DELETE su (nessuno).


[![Impostare l'elenco di elenco a discesa della scheda Inserisci metodo AddProduct](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image41.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image40.png)

**Figura 14**: impostare l'elenco di riepilogo a discesa della scheda Inserisci la `AddProduct` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image42.png))


[![Impostare l'elenco di riepilogo a discesa della scheda DELETE su (nessuno)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image44.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image43.png)

**Figura 15**: impostare l'elenco di riepilogo a discesa della scheda Elimina su (nessuno) ([fare clic per visualizzare l'immagine con dimensioni normali](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image45.png))


Dopo aver apportato queste modifiche, la sintassi dichiarativa di ObjectDataSource verrà espanso per includere un `InsertParameters` insieme, come illustrato di seguito:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample8.aspx)]

Ripetere la procedura guidata aggiunta nuovamente il `OldValuesParameterFormatString` proprietà. Si consiglia di cancellare la proprietà impostandola sul valore predefinito (`{0}`) o rimuoverlo completamente dalla sintassi dichiarativa.

Con ObjectDataSource fornendo funzionalità di inserimento, tag di DetailsView smart include ora la casella di controllo Attiva paging; tornare alla finestra di progettazione e selezionare questa opzione. Successivamente, ridurre DetailsView in modo che includa solo due BoundField - `ProductName` e `UnitPrice` - e di CommandField. Sintassi dichiarativa per DetailsView a questo punto dovrebbe essere simile:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample9.aspx)]

Figura 16 Mostra questa pagina quando viene visualizzato tramite un browser a questo punto. Come può notare, DetailsView Elenca il nome e il prezzo del primo prodotto (Chai). Ciò che vogliamo, tuttavia, è un'interfaccia di inserimento che fornisce un mezzo per l'utente di aggiungere rapidamente un nuovo prodotto di database.


[![Il controllo DetailsView è attualmente eseguito il rendering in modalità di sola lettura](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image47.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image46.png)

**Figura 16**: il controllo DetailsView è attualmente eseguito il rendering in modalità di sola lettura ([fare clic per visualizzare l'immagine con dimensioni normali](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image48.png))


Per visualizzare il controllo DetailsView nella relativa modalità di inserimento, dobbiamo impostare il `DefaultMode` proprietà `Inserting`. Ciò viene eseguito il rendering DetailsView in modalità di inserimento alla prima visita e li mantiene presenti dopo l'inserimento di un nuovo record. Come illustrato nella figura 17, tali un controllo DetailsView fornisce un'interfaccia rapida per l'aggiunta di un nuovo record.


[![DetailsView fornisce un'interfaccia per aggiungere rapidamente un nuovo prodotto](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image50.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image49.png)

**Figura 17**: DetailsView fornisce un'interfaccia per aggiungere rapidamente un nuovo prodotto ([fare clic per visualizzare l'immagine con dimensioni normali](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image51.png))


Quando l'utente immette un nome di prodotto e prezzo (ad esempio "Acqua Acme" e 1.99, come illustrato nella figura 17) e fa clic su Inserisci, un postback previsioni e inizia il flusso di lavoro di inserimento, che si conclude con un nuovo record di prodotto da aggiungere al database. DetailsView mantiene la relativa interfaccia inserimento e il controllo GridView è automaticamente riassociata alla relativa origine dati per includere il nuovo prodotto, come illustrato nella figura 18.


![Il prodotto](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image52.png)

**Figura 18**: il prodotto "Acqua Acme" è stato aggiunto al Database


Mentre il controllo GridView nella figura 18 non è evidente, i campi di prodotto darebbe origine dall'interfaccia DetailsView `CategoryID`, `SupplierID`, `QuantityPerUnit`e così via vengono assegnati `NULL` valori del database. È possibile verificarlo eseguendo i passaggi seguenti:

1. Passare a Esplora Server in Visual Studio
2. Espansione di `NORTHWND.MDF` nodo del database
3. Fare clic su di `Products` nodo della tabella di database
4. Selezionare Mostra dati tabella

Questo modo vengono elencati tutti i record nel `Products` tabella. Come nella figura 19, tutte le colonne del nostro prodotto nuovo diverso da `ProductID`, `ProductName`, e `UnitPrice` hanno `NULL` valori.


[![I campi il prodotto non fornito nel controllo DetailsView sono assegnati i valori NULL](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image54.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image53.png)

**Figura 19**: il prodotto campi non specificato nel controllo DetailsView assegnati `NULL` valori ([fare clic per visualizzare l'immagine con dimensioni normali](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image55.png))


Potrebbe essere necessario fornire un valore predefinito diverso da `NULL` per uno o più di questi valori di colonna, sia perché `NULL` non è la migliore opzione predefinita o perché non consente la colonna del database stesso `NULL` s. Per eseguire questa operazione a livello di codice è possibile impostare i valori dei parametri di DetailsView `InputParameters` raccolta. Questa assegnazione può essere eseguita sia nell'evento gestore per DetailsView `ItemInserting` evento o di ObjectDataSource `Inserting` evento. Dal momento che abbiamo già esaminato in tramite gli eventi di pre-elaborazione e post-livelli Web i dati di controllo a livello di, verranno esaminati mediante gli eventi di ObjectDataSource in questo momento.

## <a name="step-4-assigning-values-to-thecategoryidandsupplieridparameters"></a>Passaggio 4: Assegnazione di valori per il`CategoryID`e`SupplierID`parametri

Per questa esercitazione si immagini che per la nostra applicazione quando si aggiunge un nuovo prodotto tramite questa interfaccia deve essere assegnato un `CategoryID` e `SupplierID` pari a 1. Come accennato in precedenza, ObjectDataSource ha una coppia di eventi di pre-elaborazione e post-livelli generazione durante il processo di modifica dei dati. Quando relativi `Insert()` metodo viene richiamato, ObjectDataSource genera prima relativi `Inserting` evento, quindi chiama il metodo che relativo `Insert()` è stato mappato al metodo e infine genera il `Inserted` evento. Il `Inserting` gestore dell'evento mette a disposizione us un'ultima possibilità per modificare i parametri di input o annullare l'operazione definitiva.

> [!NOTE]
> In un'applicazione reale si vorrebbero per consentire all'utente specificare la categoria e il fornitore o si seleziona questo valore per alcuni criteri in base o business per la logica (anziché alla cieca selezionando un ID pari a 1). Indipendentemente dal fatto che, nell'esempio viene illustrato come impostare a livello di codice il valore del parametro di input dall'evento di pre-livello di ObjectDataSource.


Si consiglia di creare un gestore eventi per ObjectDataSource `Inserting` evento. Si noti che secondo parametro del gestore dell'evento è un oggetto di tipo `ObjectDataSourceMethodEventArgs`, che dispone di una proprietà per accedere alla raccolta di parametri (`InputParameters`) e una proprietà per annullare l'operazione (`Cancel`).


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample10.cs)]

A questo punto, il `InputParameters` proprietà contiene di ObjectDataSource `InsertParameters` insieme con i valori assegnati dalla DetailsView. Per modificare il valore di uno di questi parametri, usare semplicemente: `e.InputParameters["paramName"] = value`. Pertanto, per impostare il `CategoryID` e `SupplierID` sui valori pari a 1, modificare il `Inserting` gestore eventi all'aspetto simile al seguente:


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample11.cs)]

Questo tempo quando si aggiunge un nuovo prodotto (ad esempio Acme Soda), il `CategoryID` e `SupplierID` colonne del nuovo prodotto vengono impostate su 1 (vedere Figura 20).


[![Nuovi prodotti presentano ora CategoryID e SupplierID valori impostati su 1](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image57.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image56.png)

**Figura 20**: nuovi prodotti ora hanno Their `CategoryID` e `SupplierID` i valori impostati su 1 ([fare clic per visualizzare l'immagine con dimensioni normali](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image58.png))


## <a name="summary"></a>Riepilogo

Durante la modifica, inserimento ed eliminazione di processo, il controllo Web per dati sia ObjectDataSource procedere con un numero di eventi di pre-elaborazione e post-livelli. In questa esercitazione abbiamo esaminato gli eventi pre-livelli e stato illustrato come usarli per personalizzare i parametri di input o annullare l'operazione di modifica dei dati a entrambi completamente il controllo Web per dati e gli eventi di ObjectDataSource. Nella prossima esercitazione verrà esaminato la creazione e utilizzo dei gestori eventi per gli eventi di post-livelli.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. I revisori per questa esercitazione sono state Jackie Goor e Liz Shulok. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](an-overview-of-inserting-updating-and-deleting-data-cs.md)
> [Successivo](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md)
