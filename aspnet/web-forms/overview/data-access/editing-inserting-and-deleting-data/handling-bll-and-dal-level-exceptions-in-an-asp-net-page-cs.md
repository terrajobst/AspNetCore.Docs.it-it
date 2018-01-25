---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs
title: Gestione delle eccezioni BLL e DAL livello in una pagina ASP.NET (c#) | Documenti Microsoft
author: rick-anderson
description: "In questa esercitazione verrà spiegato come visualizzare un messaggio di errore informativo e descrittivo consigliabile si verifichi un'eccezione durante un inserimento, aggiornamento o operazione di eliminazione di..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 49d8a66c-3ea8-4087-839f-179d1d94512a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs
msc.type: authoredcontent
ms.openlocfilehash: 5a0ffde90aa85383d87bd48e16a1c16433465cbf
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="handling-bll--and-dal-level-exceptions-in-an-aspnet-page-c"></a>Gestione delle eccezioni BLL e DAL livello in una pagina ASP.NET (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare App di esempio](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_18_CS.exe) o [Scarica il PDF](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/datatutorial18cs1.pdf)

> In questa esercitazione vedremo come visualizzare un messaggio di errore informativo e descrittivo consigliabile si verifichi un'eccezione durante un inserimento, aggiornamento o operazione di eliminazione di un controllo Web di dati ASP.NET.


## <a name="introduction"></a>Introduzione

L'utilizzo di dati da un'applicazione web ASP.NET con un'architettura a più livelli applicazione implica i tre passaggi generali seguenti:

1. Determinare il metodo del livello di logica di Business deve essere richiamato e i valori di parametro per passare. I valori di parametro possono essere hardcoded, assegnato a livello di codice o input immessi dall'utente.
2. Richiamare il metodo.
3. Elaborare i risultati. Quando si chiama un metodo BLL che restituisce dati, questa operazione potrebbe comportare il data binding a un controllo Web di dati. Per i metodi BLL che modificano i dati, può trattarsi di esecuzione di un'azione in base a un valore restituito o normalmente la gestione di qualsiasi eccezione che si è verificato nel passaggio 2.

Come illustrato nel [esercitazione precedente](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md), ObjectDataSource sia dei controlli Web per forniscono punti di estendibilità per i passaggi 1 e 3. GridView, ad esempio, genera il `RowUpdating` evento prima dell'assegnazione di valori di campo di ObjectDataSource `UpdateParameters` insieme; relativo `RowUpdated` evento viene generato al termine dell'operazione ObjectDataSource.

Abbiamo già esaminato gli eventi che vengono generati durante il passaggio 1 e sono illustrato come possono essere utilizzati per personalizzare i parametri di input o annullare l'operazione. In questa esercitazione è verranno prese in esame gli eventi che attivano una volta completata l'operazione. Con questi gestori di eventi di post-livello, ad esempio, è possibile determinare se è stata generata un'eccezione durante l'operazione e gestire in modo efficiente, visualizzando un messaggio di errore informativo e descrittivo sullo schermo, anziché l'impostazione di ASP.NET standard pagina di eccezione.

Per illustrare l'utilizzo di questi eventi post-livelli, creare una pagina che elenca i prodotti in un controllo GridView. modificabile. Quando l'aggiornamento di un prodotto, se un'eccezione viene generato il nostro ASP.NET pagina verrà visualizzato un messaggio breve sopra GridView che spiega che si è verificato un problema. Iniziamo!

## <a name="step-1-creating-an-editable-gridview-of-products"></a>Passaggio 1: Creazione di un controllo GridView modificabile dei prodotti

Nell'esercitazione precedente abbiamo creato un GridView modificabile con solo due campi, `ProductName` e `UnitPrice`. Questo requisito si applica la creazione di un overload aggiuntivo per il `ProductsBLL` della classe `UpdateProduct` (metodo), uno che accettate solo tre parametri di input (nome del prodotto, prezzo unitario e ID) anziché come un parametro per ogni campo di prodotto. Per questa esercitazione pratica verrà questa tecnica, la creazione di un controllo GridView modificabile che visualizza il nome del prodotto, quantità per unità, il prezzo unitario e unità in magazzino, ma consente solo il nome, il prezzo unitario e unità in magazzino per essere modificato.

Ai fini di questo scenario è necessario un altro overload di `UpdateProduct` (metodo), uno che accetta quattro parametri: il nome del prodotto, prezzo, unità in magazzino e ID. Aggiungere il metodo seguente per la `ProductsBLL` classe:


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/samples/sample1.cs)]

Con questo metodo completato, si è pronti per creare la pagina ASP.NET che consente la modifica di questi quattro campi prodotto specifico. Aprire il `ErrorHandling.aspx` nella pagina di `EditInsertDelete` cartella e aggiungere un controllo GridView alla pagina tramite la finestra di progettazione. Associare GridView a un nuovo oggetto ObjectDataSource, mapping di `Select()` metodo il `ProductsBLL` della classe `GetProducts()` (metodo) e `Update()` metodo il `UpdateProduct` overload appena creato.


[![Utilizzare l'Overload del metodo UpdateProduct che accetta quattro parametri di Input](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image2.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image1.png)

**Figura 1**: utilizzo di `UpdateProduct` metodo di Overload che accetta quattro parametri di Input ([fare clic per visualizzare l'immagine ingrandita](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image3.png))


Verrà creato un ObjectDataSource con un `UpdateParameters` insieme con quattro parametri e un controllo GridView con un campo per ognuno dei campi del prodotto. Markup dichiarativo di ObjectDataSource assegna il `OldValuesParameterFormatString` il valore della proprietà `original_{0}`, che verrà generata un'eccezione, poiché la classe BLL non prevede un parametro di input denominato `original_productID` che deve essere passato. Non dimenticare di rimuovere completamente questa impostazione dalla sintassi dichiarativa (o impostarlo sul valore predefinito, `{0}`).

Successivamente, ridurre la quantità di GridView da includere solo la `ProductName`, `QuantityPerUnit`, `UnitPrice`, e `UnitsInStock` BoundField. È inoltre possibile applicare la formattazione a livello di campo ritengono necessari (ad esempio la modifica di `HeaderText` proprietà).

Nell'esercitazione precedente abbiamo esaminato come formattare il `UnitPrice` BoundField come valuta sia in modalità di sola lettura e la modalità di modifica. Di seguito si identico. Tenere presente che questa richiesta di impostazione del BoundField `DataFormatString` proprietà `{0:c}`, l'oggetto `HtmlEncode` proprietà `false`e il relativo `ApplyFormatInEditMode` a `true`, come illustrato nella figura 2.


[![Configurare il UnitPrice BoundField da visualizzare come valuta](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image5.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image4.png)

**Figura 2**: configurare il `UnitPrice` BoundField da visualizzare come valuta ([fare clic per visualizzare l'immagine ingrandita](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image6.png))


Formattazione di `UnitPrice` come una valuta nell'interfaccia di modifica richiede la creazione di un gestore eventi per il controllo GridView `RowUpdating` evento che analizza la stringa di formato di valuta in un `decimal` valore. Tenere presente che il `RowUpdating` gestore dell'evento dall'ultima esercitazione anche controllato per verificare che l'utente specificato un `UnitPrice` valore. Tuttavia, per questa esercitazione si consente all'utente omettere il prezzo.


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/samples/sample2.cs)]

Il GridView include un `QuantityPerUnit` BoundField, ma questo BoundField deve essere solo a scopo di visualizzazione e non deve essere modificabile dall'utente. A tale scopo, è sufficiente impostare BoundField `ReadOnly` proprietà `true`.


[![Rendere il QuantityPerUnit BoundField di sola lettura](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image8.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image7.png)

**Figura 3**: verificare il `QuantityPerUnit` BoundField Read-Only ([fare clic per visualizzare l'immagine ingrandita](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image9.png))


Infine, controllare la casella di controllo Abilita modifica smart tag del controllo GridView. Dopo aver completato questi passaggi di `ErrorHandling.aspx` finestra di progettazione della pagina simile alla figura 4.


[![Rimuovere tutto tranne necessario BoundField e verificare l'abilitazione, la casella di controllo di modifica](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image11.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image10.png)

**Figura 4**: rimuovere tutto tranne il necessario BoundField e verificare la modifica di casella di controllo ([fare clic per visualizzare l'immagine ingrandita](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image12.png))


A questo punto si dispone di un elenco di tutti i prodotti `ProductName`, `QuantityPerUnit`, `UnitPrice`, e `UnitsInStock` campi; tuttavia, solo il `ProductName`, `UnitPrice`, e `UnitsInStock` campi possono essere modificati.


[![Gli utenti possono ora facilmente modificare i nomi dei prodotti, prezzi e unità In magazzino campi](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image14.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image13.png)

**Figura 5**: nomi gli utenti possono ora facilmente modificare i prodotti, prezzi e campi di unità In magazzino ([fare clic per visualizzare l'immagine ingrandita](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image15.png))


## <a name="step-2-gracefully-handling-dal-level-exceptions"></a>Passaggio 2: Normalmente la gestione delle eccezioni di livello DAL

Durante il GridView modificabile estesamente quando gli utenti immettere i valori validi per nome modificato del prodotto, prezzo e unità in magazzino, immettere i valori non validi genera un'eccezione. Ad esempio, omettendo il `ProductName` valore, un [NoNullAllowedException](https://msdn.microsoft.com/library/default.asp?url=/library/cpref/html/frlrfsystemdatanonullallowedexceptionclasstopic.asp) generata dal `ProductName` proprietà nel `ProdcutsRow` classe dispone di relativo `AllowDBNull` proprietà impostata su `false`; se il database non è attivo, un `SqlException` verrà generato dal TableAdapter quando si tenta di connettersi al database. Senza eseguire alcuna operazione, queste eccezioni propagata dal livello di accesso ai dati per il livello di logica di Business, quindi nella pagina ASP.NET e infine al runtime di ASP.NET.

A seconda della configurazione dell'applicazione web e se si sta visitando l'applicazione da `localhost`, può causare un'eccezione non gestita in una pagina di errore server generico, un report di errore dettagliati o una semplice pagina web. Vedere [Error Handling applicazione Web in ASP.NET](http://www.15seconds.com/issue/030102.htm) e [elemento customErrors](https://msdn.microsoft.com/library/h0hfz6fc(VS.80).aspx) per ulteriori informazioni su come il runtime ASP.NET risponde a un'eccezione non rilevata.

La figura 6 mostra una schermata durante il tentativo di aggiornare un prodotto senza specificare il `ProductName` valore. L'impostazione predefinita il report di errore dettagliato visualizzato quando tramite `localhost`.


[![L'omissione di dettagli di eccezione verrà visualizzato nome del prodotto](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image17.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image16.png)

**Figura 6**: l'omissione di dettagli del prodotto nome verrà visualizzato eccezione ([fare clic per visualizzare l'immagine ingrandita](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image18.png))


Anche se tali dettagli eccezione sono utili durante il test di un'applicazione, la presentazione di un utente finale con schermo in caso di un'eccezione è minore di ideale. Un utente finale che non riconosce un `NoNullAllowedException` è o perché è stato creato. Un approccio migliore consiste nel presentare all'utente un messaggio più semplice che indica che si sono verificati problemi durante il tentativo di aggiornare il prodotto.

Se si verifica un'eccezione quando si esegue l'operazione, gli eventi post-livelli ObjectDataSource sia il controllo Web di dati forniscono un mezzo per il rilevamento e annullare la generazione dell'eccezione bubbling verso il runtime di ASP.NET. Per questo esempio, creare un gestore eventi per il controllo GridView `RowUpdated` evento che determina se un'eccezione generato e, in caso affermativo, consente di visualizzare i dettagli dell'eccezione in un controllo etichetta Web.

Per iniziare, aggiungere un'etichetta per la pagina ASP.NET, l'impostazione relativa `ID` proprietà `ExceptionDetails` e cancellare la `Text` proprietà. Per disegnare occhio dell'utente a questo messaggio, impostare il relativo `CssClass` proprietà `Warning`, che è una classe CSS è stato aggiunto al `Styles.css` file nell'esercitazione precedente. Tenere presente che questa classe CSS fa sì che il testo dell'etichetta da visualizzare in un tipo di carattere rosso, corsivo, grassetto, molto grande.


[![Aggiungere un controllo Web etichetta alla pagina](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image20.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image19.png)

**Figura 7**: aggiungere un controllo Web etichetta ([fare clic per visualizzare l'immagine ingrandita](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image21.png))


Poiché si desidera che questo controllo per essere visibile solo immediatamente dopo un'eccezione, impostare il relativo `Visible` proprietà su false nella `Page_Load` gestore eventi:


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/samples/sample3.cs)]

Con questo codice, nella prima visita di pagina e postback successivi il `ExceptionDetails` controllo avrà relativo `Visible` proprietà impostata su `false`. In caso di un'eccezione, a livello BLL o DAL quale è possibile rilevare in GridView `RowUpdated` gestore eventi, si imposterà il `ExceptionDetails` del controllo `Visible` proprietà su true. Poiché i gestori di eventi di controllo Web si verificano dopo il `Page_Load` gestore eventi del ciclo di vita di pagina, verrà visualizzata l'etichetta. Tuttavia, nei postback successivi, il `Page_Load` gestore dell'evento verrà ripristinato il `Visible` proprietà nuovamente a `false`, nasconderla dalla vista.

> [!NOTE]
> In alternativa, è stato possibile rimuovere la necessità per l'impostazione di `ExceptionDetails` del controllo `Visible` proprietà in `Page_Load` assegnando relativo `Visible` proprietà `false` la sintassi dichiarativa e la disabilitazione di stato di visualizzazione (l'impostazione relativa `EnableViewState` proprietà `false`). Questa alternativa verrà utilizzata in un'esercitazione future.


Con il controllo etichetta aggiunto, il passaggio successivo consiste nel creare il gestore dell'evento per il controllo GridView `RowUpdated` evento. Selezionare il controllo GridView nella finestra di progettazione, Vai alla finestra proprietà e scegliere l'icona del fulmine, che elenca gli eventi del controllo GridView. Deve già esistere una voce per il controllo GridView `RowUpdating` evento, come un gestore eventi per questo evento è stato creato in precedenza in questa esercitazione. Creare un gestore eventi per il `RowUpdated` anche l'evento.


![Creare un gestore eventi per l'evento RowUpdated di GridView](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image22.png)

**Figura 8**: creare un gestore eventi per il controllo GridView `RowUpdated` evento


> [!NOTE]
> È anche possibile creare il gestore eventi tramite gli elenchi a discesa nella parte superiore del file di classe code-behind. Selezionare il controllo GridView dall'elenco a discesa a sinistra e `RowUpdated` evento rispetto a quella a destra.


Creazione di questo gestore eventi aggiungerà il codice seguente in una classe code-behind della pagina ASP.NET:


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/samples/sample4.cs)]

Secondo parametro di input del gestore di questo evento è un oggetto di tipo [GridViewUpdatedEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewupdatedeventargs.aspx), che dispone di tre proprietà di interesse per la gestione delle eccezioni:

- `Exception`un riferimento all'eccezione generata. Se è non stata generata alcuna eccezione, questa proprietà sarà assegnato un valore di`null`
- `ExceptionHandled`un valore booleano che indica se l'eccezione è stata gestita nel `RowUpdated` gestore; se `false` (impostazione predefinita), l'eccezione viene generata nuovamente, percolating al runtime ASP.NET
- `KeepInEditMode`Se impostato su `true` riga GridView modificata rimane in modalità di modifica; se `false` (impostazione predefinita), la riga GridView, vengono ripristinate la modalità di sola lettura

Il codice, quindi, deve verificare se `Exception` non `null`, vale a dire che è stata generata un'eccezione durante l'operazione. In questo caso, si vuole:

- Viene visualizzato un messaggio descrittivo nel `ExceptionDetails` etichetta
- Indicare che è stata gestita l'eccezione
- Mantenere la riga GridView in modalità di modifica

Il codice seguente consente di realizzare questi obiettivi:


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/samples/sample5.cs)]

Questo gestore dell'evento inizia con un controllo per verificare se `e.Exception` è `null`. In caso contrario, il `ExceptionDetails` dell'etichetta `Visible` è impostata su `true` e il relativo `Text` proprietà su "Si è verificato un problema durante l'aggiornamento del prodotto." I dettagli di cui è stata generata l'eccezione si trovano nel `e.Exception` dell'oggetto `InnerException` proprietà. Viene esaminata l'eccezione interna e, in caso di un determinato tipo, viene accodato un messaggio aggiuntivo utile il `ExceptionDetails` dell'etichetta `Text` proprietà. Infine, il `ExceptionHandled` e `KeepInEditMode` sono entrambe impostate su `true`.

Figura 9 è illustrata una cattura di schermata della pagina quando si omette il nome del prodotto. Figura 10 Mostra i risultati quando si immette un valido `UnitPrice` valore (-50).


[![BoundField il ProductName deve contenere un valore](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image24.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image23.png)

**Figura 9**: il `ProductName` BoundField deve contenere un valore ([fare clic per visualizzare l'immagine ingrandita](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image25.png))


[![I valori negativi UnitPrice non sono consentito](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image27.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image26.png)

**Figura 10**: negativo `UnitPrice` i valori non sono consentito ([fare clic per visualizzare l'immagine ingrandita](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image28.png))


Impostando il `e.ExceptionHandled` proprietà `true`, `RowUpdated` gestore eventi è indicato che l'eccezione è gestita. Pertanto, l'eccezione non propaga al runtime di ASP.NET.

> [!NOTE]
> Figure 9 e 10 è illustrato un modo per gestire le eccezioni generate a causa di input dell'utente non valido. In teoria, tuttavia, questo tipo di input non validi non verrà mai raggiungere il livello di logica di Business in primo luogo, come la pagina ASP.NET è necessario assicurarsi che gli input dell'utente siano validi prima di richiamare il `ProductsBLL` della classe `UpdateProduct` metodo. Nell'esercitazione successiva che vedremo come aggiungere i controlli di convalida per le interfacce di modifica e inserimento per garantire che i dati inviati al livello di logica di Business è conforme alle regole di business. I controlli di convalida non solo impediscono la chiamata del `UpdateProduct` metodo fino a quando i dati forniti dall'utente sono validi, ma anche forniscono un'esperienza utente più informativa per l'identificazione di problemi nell'immissione dati.


## <a name="step-3-gracefully-handling-bll-level-exceptions"></a>Passaggio 3: Normalmente la gestione delle eccezioni di livello BLL

Durante l'inserimento, aggiornamento o eliminazione di dati, il livello di accesso ai dati può generare un'eccezione in caso di un errore di dati correlati. Il database potrebbe essere offline, una colonna di tabella di database richiesti non potrebbe avere avuto un valore specificato o un vincolo a livello di tabella sono stato violato. Oltre alle eccezioni strettamente correlate a dati, il livello di logica di Business è possibile utilizzare le eccezioni per indicare quando siano state violate le regole di business. Nel [la creazione di un livello di logica di Business](../introduction/creating-a-business-logic-layer-cs.md) esercitazione, ad esempio, abbiamo aggiunto una verifica della regola business all'originale `UpdateProduct` rapporto di overload. In particolare, se l'utente è stato contrassegnato un prodotto come non più disponibile, è necessario che il prodotto non deve essere l'unica fornito dal relativo fornitore. Se questa condizione è stata violata, un `ApplicationException` è stata generata.

Per il `UpdateProduct` overload creata in questa esercitazione, aggiungere una regola business che impedisce il `UnitPrice` campo viene impostata su un nuovo valore che è più del doppio rispetto all'originale `UnitPrice` valore. A tale scopo, modificare il `UpdateProduct` overload in modo che esegue questo controllo e genera un `ApplicationException` se la regola viene violata. Il metodo aggiornato di seguito:


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/samples/sample6.cs)]

Con questa modifica, qualsiasi aggiornamento prezzo che è più del doppio rispetto al prezzo esistente comporta un `ApplicationException` generata. Come l'eccezione generata DAL, questo generato BLL `ApplicationException` può essere rilevato e gestito in GridView `RowUpdated` gestore dell'evento. In realtà, il `RowUpdated` codice del gestore eventi, come è scritto, correttamente rileverà questa eccezione e visualizzerà il `ApplicationException`del `Message` valore della proprietà. Figura 11 mostra una schermata quando un utente tenta di aggiornare il prezzo di Chai $50,00, ovvero più del doppio il prezzo corrente di $19,95.


[![Le regole di Business di impedire l'aumento di prezzo che più del doppio prezzo del prodotto](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image30.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image29.png)

**Figura 11**: aumenti di prezzo non consentire che più del doppio prezzo del prodotto delle regole di Business ([fare clic per visualizzare l'immagine ingrandita](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image31.png))


> [!NOTE]
> Idealmente sarebbero il refactoring nostri le regole business dal `UpdateProduct` overload del metodo e in un metodo comune. Questo viene lasciato come esercizio per il lettore.


## <a name="summary"></a>Riepilogo

Durante l'inserimento, aggiornamento e operazioni di eliminazione, sia i dati Web e il controllo ObjectDataSource coinvolti generare eventi di pre e post-livelli che l'operazione effettiva di delimitatore. Come è stato illustrato in questa esercitazione e quello precedente, quando si lavora con un GridView modificabile del controllo GridView `RowUpdating` viene generato l'evento, seguito da ObjectDataSource `Updating` evento, a quel punto il comando di aggiornamento viene effettuato il ObjectDataSource oggetto sottostante. Dopo il completamento dell'operazione, di ObjectDataSource `Updated` viene generato l'evento, seguito da GridView `RowUpdated` evento.

È possibile creare gestori eventi per gli eventi di pre-livelli per personalizzare i parametri di input o per gli eventi post-livelli per controllare e rispondere ai risultati dell'operazione. I gestori eventi post-livello sono più comunemente utilizzati per rilevare se è stata generata un'eccezione durante l'operazione. In caso di un'eccezione, questi gestori eventi post-livello, facoltativamente, è stato in grado di gestire l'eccezione in modo autonomo. In questa esercitazione è stato illustrato come gestire tale eccezione visualizzando un messaggio di errore descrittivo.

Nella prossima esercitazione si vedrà come ridurre la probabilità di eccezioni derivanti da problemi di formattazione dei dati (quali l'immissione di un valore negativo `UnitPrice`). In particolare, verrà esaminato come aggiungere i controlli di convalida per le interfacce di modificare e l'inserimento.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore per questa esercitazione è stata Liz Shulok. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Precedente](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md)
[Successivo](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md)
