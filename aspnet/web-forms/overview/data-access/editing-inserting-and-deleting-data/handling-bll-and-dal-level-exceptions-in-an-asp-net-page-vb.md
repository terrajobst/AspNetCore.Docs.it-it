---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb
title: Gestione delle eccezioni a livello BLL e in una pagina ASP.NET (VB) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione si vedrà come visualizzare un messaggio di errore descrittivo e informativi deve verificarsi un'eccezione durante un inserimento, aggiornamento o operazione di eliminazione di...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 129d4338-1315-4f40-89b5-2b84b807707d
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb
msc.type: authoredcontent
ms.openlocfilehash: 41a0ec0db4cb2093d7fadd2de7e65ee4874f0063
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394683"
---
<a name="handling-bll--and-dal-level-exceptions-in-an-aspnet-page-vb"></a>Gestione delle eccezioni a livello BLL e in una pagina ASP.NET (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'App di esempio](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_18_VB.exe) o [Scarica il PDF](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/datatutorial18vb1.pdf)

> In questa esercitazione si vedrà come visualizzare un messaggio di errore descrittivo e informativi deve verificarsi un'eccezione durante un inserimento, aggiornamento o operazione di eliminazione di un controllo Web per dati ASP.NET.


## <a name="introduction"></a>Introduzione

Utilizzo dei dati da un'applicazione web ASP.NET usando un'architettura di applicazioni a più livelli prevede i tre passaggi generali seguenti:

1. Determinare quale metodo di livello della logica di Business deve essere richiamato e i valori di parametro per passarla. I valori dei parametri possono essere hardcoded, assegnato a livello di codice o gli input immessi dall'utente.
2. Richiamare il metodo.
3. Elaborare i risultati. Quando si chiama un metodo BLL che restituisce dati, questa operazione potrebbe comportare l'associazione dei dati in un controllo Web di dati. Per i metodi di livello BLL che modificano i dati, ad esempio eseguendo un'azione basata su un valore restituito o normalmente la gestione di qualsiasi eccezione che si è verificato nel passaggio 2.

Come abbiamo visto nel [esercitazione precedente](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md), sia ObjectDataSource e i controlli Web dei dati forniscono i punti di estendibilità per i passaggi 1 e 3. Il controllo GridView, ad esempio, genera relativo `RowUpdating` evento prima dell'assegnazione i valori dei campi di ObjectDataSource `UpdateParameters` raccolta; relativo `RowUpdated` evento viene generato al termine dell'operazione di ObjectDataSource.

Abbiamo già esaminato gli eventi che si attivano durante il passaggio 1 e avranno visto come possono essere utilizzati per personalizzare i parametri di input o annullare l'operazione. In questa esercitazione si sarà concentrare l'attenzione agli eventi che vengono generati dopo l'operazione è stata completata. In questi gestori di evento di post-livello che tra le altre cose, è possibile, determinare se si è verificata un'eccezione durante l'operazione e gestire in modo efficiente, visualizzazione di un messaggio di errore descrittivo informativo sullo schermo piuttosto che verrà utilizzato da ASP.NET standard pagina delle eccezioni.

Per illustrare l'utilizzo di questi eventi post-livelli, è possibile creare una pagina che elenca i prodotti in un controllo GridView modificabile. Quando l'aggiornamento di un prodotto, se un'eccezione viene generata l'ASP.NET pagina visualizzerà un messaggio breve sopra il controllo GridView che spiega che si è verificato un problema. Iniziamo!

## <a name="step-1-creating-an-editable-gridview-of-products"></a>Passaggio 1: Creazione di un controllo GridView modificabile dei prodotti

Nell'esercitazione precedente è stato creato un GridView modificabile con solo due campi, `ProductName` e `UnitPrice`. Ciò richiesto la creazione di un overload aggiuntivo per il `ProductsBLL` della classe `UpdateProduct` (metodo), uno che accettato solo tre parametri di input (nome del prodotto, prezzo unitario e ID) anziché come un parametro per ogni campo prodotto. Per questa esercitazione, è possibile provare questa tecnica anche in questo caso, la creazione di un controllo GridView modificabile che visualizza il nome del prodotto, quantity per ogni unità, prezzo unitario e unità in magazzino, ma permette solo il nome, prezzo unitario e unità in magazzino per essere modificato.

Per supportare questo scenario è necessario un altro overload della funzione di `UpdateProduct` (metodo), uno che accetta quattro parametri: il nome del prodotto, prezzo unit, unità in magazzino e l'ID. Aggiungere il metodo seguente per il `ProductsBLL` classe:


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample1.vb)]

Con questo metodo completato, siamo pronti per creare la pagina ASP.NET che consente la modifica di questi quattro campi di quel particolare prodotto. Aprire il `ErrorHandling.aspx` nella pagina di `EditInsertDelete` cartella e aggiungere un controllo GridView alla pagina tramite la finestra di progettazione. Associare il controllo GridView per un nuovo oggetto ObjectDataSource, mapping di `Select()` metodo per il `ProductsBLL` della classe `GetProducts()` (metodo) e il `Update()` metodo per il `UpdateProduct` overload appena creato.


[![Usare l'Overload del metodo UpdateProduct che accetta quattro parametri di Input](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image2.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image1.png)

**Figura 1**: usare il `UpdateProduct` metodo di Overload che accetta quattro parametri di Input ([fare clic per visualizzare l'immagine con dimensioni normali](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image3.png))


Verrà creato un oggetto ObjectDataSource con una `UpdateParameters` insieme con quattro parametri e un controllo GridView con un campo per ogni campo prodotto. Markup dichiarativo di ObjectDataSource assegna il `OldValuesParameterFormatString` il valore della proprietà `original_{0}`, che genererà un'eccezione poiché la classe di livello BLL non prevede un parametro di input denominato `original_productID` che deve essere passato. Non dimenticare di rimuovere completamente questa impostazione dalla sintassi dichiarativa (o impostarlo sul valore predefinito, `{0}`).

Successivamente, ridurre la quantità di GridView per includere solo le `ProductName`, `QuantityPerUnit`, `UnitPrice`, e `UnitsInStock` BoundField. È inoltre possibile applicare la formattazione a livello di campo ritengono necessari (ad esempio la modifica di `HeaderText` proprietà).

Nell'esercitazione precedente abbiamo esaminato come formattare il `UnitPrice` BoundField come valuta in modalità sola lettura e la modalità di modifica. È possibile eseguire la stessa qui. È importante ricordare che questa impostazione obbligatoria del BoundField `DataFormatString` proprietà `{0:c}`, la relativa `HtmlEncode` proprietà `false`e il relativo `ApplyFormatInEditMode` a `true`, come illustrato nella figura 2.


[![Configurare i BoundField di UnitPrice da visualizzare come una valuta](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image5.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image4.png)

**Figura 2**: configurare il `UnitPrice` BoundField da visualizzare come una valuta ([fare clic per visualizzare l'immagine con dimensioni normali](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image6.png))


Formattazione di `UnitPrice` come una valuta nell'interfaccia di modifica richiede la creazione di un gestore eventi per il controllo GridView `RowUpdating` evento che analizza la stringa in formato valuta un `decimal` valore. Si tenga presente che il `RowUpdating` gestore dell'evento dall'ultima esercitazione anche controllato per verificare che l'utente fornito un `UnitPrice` valore. Tuttavia, per questa esercitazione si consente all'utente di omettere il prezzo.


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample2.vb)]

I GridView include un `QuantityPerUnit` BoundField, ma questo BoundField deve essere solo per scopi di visualizzazione e non deve essere modificabile dall'utente. A tale scopo, è sufficiente impostare BoundField `ReadOnly` proprietà `true`.


[![Rendere i QuantityPerUnit BoundField di sola lettura](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image8.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image7.png)

**Figura 3**: verificare la `QuantityPerUnit` BoundField Read-Only ([fare clic per visualizzare l'immagine con dimensioni normali](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image9.png))


Infine, selezionare la casella di controllo Abilita modifica dallo smart tag del controllo GridView. Dopo aver completato questi passaggi di `ErrorHandling.aspx` progettazione della pagina dovrebbe essere simile alla figura 4.


[![Rimuovi tutto tranne i necessari BoundField e verificare di abilitare la casella di controllo di modifica](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image11.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image10.png)

**Figura 4**: rimuovere tutto tranne la necessaria BoundField e verificare la modifica di casella di controllo ([fare clic per visualizzare l'immagine con dimensioni normali](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image12.png))


A questo punto è disponibile un elenco di tutti i prodotti `ProductName`, `QuantityPerUnit`, `UnitPrice`, e `UnitsInStock` campi; tuttavia, solo il `ProductName`, `UnitPrice`, e `UnitsInStock` campi possono essere modificati.


[![Gli utenti ora possono modificare facilmente i nomi dei prodotti, i prezzi e unità In magazzino campi](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image14.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image13.png)

**Figura 5**: gli utenti possono ora facilmente modificare prodotti nomi, i prezzi e unità In magazzino campi ([fare clic per visualizzare l'immagine con dimensioni normali](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image15.png))


## <a name="step-2-gracefully-handling-dal-level-exceptions"></a>Passaggio 2: Normalmente la gestione delle eccezioni a livello DAL

Anche se il controllo GridView modificabile funziona incredibile quando gli utenti immettono i valori validi per nome modificato del prodotto, prezzo e unità in magazzino, immettendo i valori non validi genera un'eccezione. Ad esempio, l'omissione il `ProductName` valore, un [NoNullAllowedException](https://msdn.microsoft.com/library/default.asp?url=/library/cpref/html/frlrfsystemdatanonullallowedexceptionclasstopic.asp) generata dal `ProductName` proprietà nel `ProdcutsRow` classe dispone di relativo `AllowDBNull` proprietà impostata su `false`; se il database non è attivo, un `SqlException` verrà generata dal TableAdapter quando si prova a connettersi al database. Senza eseguire alcuna azione, queste eccezioni intercetterà dal livello di accesso ai dati a livello della logica di Business, quindi nella pagina ASP.NET e infine al runtime di ASP.NET.

A seconda del modo in cui l'applicazione web viene configurata e se si sta visitando l'applicazione da `localhost`, può causare un'eccezione non gestita in una pagina di errore server generico, un report di errore dettagliati o una pagina web semplici da usare. Visualizzare [Error Handling dell'applicazione Web ASP.NET](http://www.15seconds.com/issue/030102.htm) e il [elemento customErrors](https://msdn.microsoft.com/library/h0hfz6fc(VS.80).aspx) per altre informazioni sul modo in cui il runtime ASP.NET risponde a un'eccezione non rilevata.

Figura 6 mostra la schermata durante il tentativo di aggiornare un prodotto senza specificare il `ProductName` valore. Questo è il valore predefinito di errore dettagliato report visualizzato quando si passa attraverso `localhost`.


[![L'omissione di dettagli di eccezione verrà visualizzato nome del prodotto](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image17.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image16.png)

**Figura 6**: l'omissione di dettagli del prodotto nome verrà visualizzato eccezione ([fare clic per visualizzare l'immagine con dimensioni normali](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image18.png))


Anche se tali informazioni dettagliate sull'eccezione sono utili durante il test di un'applicazione, presentando un utente finale con questo tipo una schermata in caso di un'eccezione è inferiore a quello ideale. Un utente finale probabilmente non riconosce un `NoNullAllowedException` è o perché è stato creato. Un approccio migliore è offrire all'utente un messaggio più semplici da usare che spiega che si sono verificati problemi durante il tentativo di aggiornare il prodotto.

Se si verifica un'eccezione quando si esegue l'operazione, gli eventi post-livelli in ObjectDataSource e il controllo Web per dati forniscono un mezzo per individuarlo e annullare la generazione dell'eccezione bubbling verso il runtime ASP.NET. Per questo esempio, creiamo un gestore eventi per il controllo GridView `RowUpdated` evento che determina se le ha generato un'eccezione e, in caso affermativo, Visualizza i dettagli dell'eccezione in un controllo etichetta Web.

Iniziare aggiungendo un'etichetta per la pagina ASP.NET, l'impostazione relativa `ID` proprietà `ExceptionDetails` e cancellare relativo `Text` proprietà. Per attirare l'attenzione dell'utente su questo messaggio, impostare relativo `CssClass` proprietà `Warning`, ovvero una classe CSS è stato aggiunto al `Styles.css` file nell'esercitazione precedente. Tenere presente che questa classe CSS fa sì che il testo dell'etichetta da visualizzare in un tipo di carattere rosso, corsivo, grassetto, molto grande.


[![Aggiungere un controllo Web etichetta alla pagina](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image20.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image19.png)

**Figura 7**: aggiungere un controllo Web etichetta alla pagina ([fare clic per visualizzare l'immagine con dimensioni normali](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image21.png))


Poiché si desidera che questo controllo etichetta Web siano visibili solo immediatamente dopo che si è verificata un'eccezione, impostare relativi `Visible` su false nella proprietà di `Page_Load` gestore dell'evento:


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample3.vb)]

Con questo codice, sulla prima pagina visita e durante i postback successivi il `ExceptionDetails` controllo avrà relativi `Visible` impostata su `false`. In caso di un'eccezione DAL - o a livello BLL, che sia possibile rilevare in GridView `RowUpdated` gestore eventi, si imposterà il `ExceptionDetails` del controllo `Visible` proprietà su true. Poiché i gestori eventi di controllo Web si verificano dopo il `Page_Load` gestore eventi del ciclo di vita di pagina, verrà visualizzato l'etichetta. Tuttavia, con il successivo postback, il `Page_Load` gestore eventi verrà ripristinato il `Visible` proprietà verso `false`, nasconderlo dalla visualizzazione.

> [!NOTE]
> In alternativa, è stato possibile rimuovere la necessità per l'impostazione di `ExceptionDetails` del controllo `Visible` proprietà nel `Page_Load` assegnando relativo `Visible` proprietà `false` nella sintassi dichiarativa e la disabilitazione di stato di visualizzazione (l'impostazione relativa `EnableViewState` proprietà `false`). Si userà questo approccio alternativo in un'esercitazione futura.


Con il controllo etichetta aggiunta, il passaggio successivo consiste nel creare il gestore eventi per il controllo GridView `RowUpdated` evento. Selezionare il controllo GridView nella finestra di progettazione, passare alla finestra delle proprietà e scegliere l'icona a forma di fulmine, elencare gli eventi del controllo GridView. Non vi sarà già presente una voce per il controllo GridView `RowUpdating` evento, come un gestore eventi per questo evento viene creato precedentemente in questa esercitazione. Creare un gestore eventi per il `RowUpdated` evento nonché.


![Creare un gestore eventi per l'evento RowUpdated di GridView](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image22.png)

**Figura 8**: creare un gestore eventi per il controllo GridView `RowUpdated` evento


> [!NOTE]
> È anche possibile creare il gestore eventi tramite gli elenchi a discesa nella parte superiore del file di classe code-behind. Selezionare il controllo GridView nell'elenco a discesa a sinistra e `RowUpdated` evento rispetto a quella a destra.


Creazione di questo gestore eventi verrà aggiunto il codice seguente alla classe code-behind della pagina ASP.NET:


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample4.vb)]

Secondo parametro di questo gestore dell'evento è un oggetto di tipo [GridViewUpdatedEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewupdatedeventargs.aspx), che dispone di tre proprietà di interesse per la gestione delle eccezioni:

- `Exception` un riferimento all'eccezione generata; Se è non stata generata alcuna eccezione, questa proprietà sarà assegnato un valore di `null`
- `ExceptionHandled` valore booleano che indica se l'eccezione è stata gestita nel `RowUpdated` gestore dell'evento; se `false` (impostazione predefinita), l'eccezione viene generata nuovamente, percolating al runtime di ASP.NET
- `KeepInEditMode` Se impostato su `true` riga GridView modificata rimane in modalità di modifica; se `false` (impostazione predefinita), la riga GridView Ripristina la modalità di sola lettura

Il codice, quindi, deve verificare se `Exception` non è `null`, vale a dire che è stata generata un'eccezione durante l'operazione. In questo caso, è necessario:

- Visualizzare un messaggio descrittivo nel `ExceptionDetails` etichetta
- Indicare che è stata gestita l'eccezione
- Mantenere la riga GridView in modalità di modifica

Il codice seguente consente di realizzare questi obiettivi:


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample5.vb)]

Questo gestore dell'evento inizia con un controllo per verificare se `e.Exception` è `null`. In caso contrario, il `ExceptionDetails` dell'etichetta `Visible` è impostata su `true` e il relativo `Text` proprietà su "Si è verificato un problema durante l'aggiornamento del prodotto." I dettagli dell'eccezione effettivo che è stata generata un'eccezione si trovano nel `e.Exception` dell'oggetto `InnerException` proprietà. Questa eccezione interna viene esaminata e, se è di un determinato tipo, viene aggiunto un messaggio aggiuntivo, utile per la `ExceptionDetails` dell'etichetta `Text` proprietà. Infine, il `ExceptionHandled` e `KeepInEditMode` sono entrambe impostate su `true`.

Figura 9 mostra una cattura di schermata della pagina quando si omette il nome del prodotto. Figura 10 Mostra i risultati quando si immette un valido `UnitPrice` valore (-50).


[![I ProductName BoundField deve contenere un valore](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image24.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image23.png)

**Figura 9**: il `ProductName` BoundField deve contenere un valore ([fare clic per visualizzare l'immagine con dimensioni normali](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image25.png))


[![I valori negativi UnitPrice non sono consentito](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image27.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image26.png)

**Figura 10**: negativa `UnitPrice` i valori non sono consentito ([fare clic per visualizzare l'immagine con dimensioni normali](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image28.png))


Impostando il `e.ExceptionHandled` proprietà `true`, il `RowUpdated` gestore dell'evento ha indicato che ha gestito l'eccezione. Pertanto, l'eccezione non propaga fino al runtime ASP.NET.

> [!NOTE]
> Le figure da 9 e 10 viene illustrato un modo per gestire le eccezioni generate a causa di un input dell'utente non è valido. In teoria, tuttavia, questo tipo di input non valido non verrà mai raggiungere il livello di logica di Business in primo luogo, come la pagina ASP.NET è necessario assicurarsi che gli input dell'utente siano validi prima di richiamare il `ProductsBLL` della classe `UpdateProduct` (metodo). Nell'esercitazione successiva che si vedrà come aggiungere i controlli di convalida alle interfacce di modifica e inserimento per garantire che i dati inviati al livello della logica di Business è conforme alle regole di business. I controlli di convalida non solo impediscono la chiamata del `UpdateProduct` metodo fino a quando i dati specificati dall'utente sono validi, ma anche forniscono un'esperienza utente maggiori per l'identificazione di problemi nell'immissione dati.


## <a name="step-3-gracefully-handling-bll-level-exceptions"></a>Passaggio 3: Normalmente la gestione delle eccezioni a livello BLL

Durante l'inserimento, aggiornamento o eliminazione di dati, il livello di accesso ai dati può generare un'eccezione in caso di un errore relativo a dati. Il database potrebbe essere offline, una colonna della tabella di database necessari potrebbe non avere avuto un valore specificato o un vincolo a livello di tabella sono stato violato. Oltre alle eccezioni strettamente correlate ai dati, il livello di logica di Business è possibile usare le eccezioni per indicare quando le regole di business sono state violate. Nel [creazione di un livello di logica di Business](../introduction/creating-a-business-logic-layer-vb.md) esercitazione, ad esempio, abbiamo aggiunto una verifica della regola business all'istanza originale `UpdateProduct` rapporto di overload. In particolare, se l'utente è stato contrassegnato un prodotto come non più disponibile, è necessario che il prodotto non deve essere l'unico fornito dal relativo fornitore. Se questa condizione è stata violata, un `ApplicationException` è stata generata.

Per il `UpdateProduct` overload creata in questa esercitazione, si aggiungerà una regola business che impedisce il `UnitPrice` campo impostato su un nuovo valore che è più del doppio originale `UnitPrice` valore. A tale scopo, modificare il `UpdateProduct` overload in modo che esegue questo controllo e genera un `ApplicationException` se la regola viene violata. Il metodo di aggiornamento seguente:


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample6.vb)]

Con questa modifica, qualsiasi aggiornamento prezzo più di due volte il prezzo esistente causerà un `ApplicationException` generata. Esattamente come l'eccezione generata DAL, questo ha generato BLL `ApplicationException` può essere rilevato e gestito del controllo GridView `RowUpdated` gestore dell'evento. In effetti, il `RowUpdated` codice del gestore eventi, come scritto, in modo corretto rileverà questa eccezione e visualizzerà le `ApplicationException`del `Message` valore della proprietà. Figura 11 mostra una schermata quando un utente tenta di aggiornare il prezzo di Chai compare a $50,00, ovvero più del doppio essa il prezzo corrente di 19,95 dollari con acquisto.


[![Le regole di Business non consentire l'aumento dei prezzi che più del doppio prezzo del prodotto](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image30.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image29.png)

**Figura 11**: aumento dei prezzi non consentire che più del doppio prezzo del prodotto delle regole di Business ([fare clic per visualizzare l'immagine con dimensioni normali](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image31.png))


> [!NOTE]
> In teoria le regole per la logica di business potrebbero eseguire il refactoring fuori il `UpdateProduct` overload del metodo e in un metodo comune. Ciò viene svolta come esercizio per il lettore.


## <a name="summary"></a>Riepilogo

Durante l'inserimento, aggiornamento ed eliminazione di operazioni, sia il controllo Web per dati e ObjectDataSource coinvolte vengono generati gli eventi di pre-elaborazione e post-livelli tale delimitatore l'operazione effettiva. Come abbiamo visto in questa esercitazione e quello precedente, quando si lavora con un GridView modificabile del controllo GridView `RowUpdating` viene generato l'evento, seguita da ObjectDataSource `Updating` evento, a questo punto viene eseguito il comando di aggiornamento di ObjectDataSource oggetto sottostante. Dopo il completamento dell'operazione, di ObjectDataSource `Updated` viene generato l'evento, seguita da GridView `RowUpdated` evento.

È possibile creare gestori eventi per gli eventi di pre-livelli per personalizzare i parametri di input o per gli eventi di post-livelli per esaminare e rispondere ai risultati dell'operazione. I gestori di evento di post-livello utilizzati più frequentemente per rilevare se si è verificata un'eccezione durante l'operazione. In caso di un'eccezione, questi gestori eventi post-livello, facoltativamente, è stato in grado di gestire l'eccezione in modo indipendente. In questa esercitazione è stato illustrato come gestire tale eccezione visualizzando un messaggio di errore descrittivo.

Nella prossima esercitazione verrà illustrato come per ridurre la probabilità che le eccezioni derivanti da problemi di formattazione dei dati (ad esempio immettendo un valore negativo `UnitPrice`). In particolare, esamineremo come aggiungere i controlli di convalida alle interfacce di modifica e inserimento.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore capo di questa esercitazione è stata Liz Shulok. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)
> [Successivo](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md)
