---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/handling-bll-and-dal-level-exceptions-vb
title: Gestione delle eccezioni BLL e DAL livello (VB) | Documenti Microsoft
author: rick-anderson
description: In questa esercitazione, vedremo come tactfully gestire le eccezioni generate durante l'aggiornamento flusso di lavoro di DataList un modificabile.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: ca665073-b379-4239-9404-f597663ca65e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/handling-bll-and-dal-level-exceptions-vb
msc.type: authoredcontent
ms.openlocfilehash: 0ee0eeff08b6ba490b401540de833eecd7122a17
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30880102"
---
<a name="handling-bll--and-dal-level-exceptions-vb"></a>Gestione delle eccezioni BLL e DAL livello (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare App di esempio](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_38_VB.exe) o [Scarica il PDF](handling-bll-and-dal-level-exceptions-vb/_static/datatutorial38vb1.pdf)

> In questa esercitazione, vedremo come tactfully gestire le eccezioni generate durante l'aggiornamento flusso di lavoro di DataList un modificabile.


## <a name="introduction"></a>Introduzione

Nel [Panoramica della modifica e l'eliminazione di dati in DataList](an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md) esercitazione, è stato creato un controllo DataList che offerti semplice eliminazione e modifica di funzionalità. Mentre pienamente funzionali, era difficilmente descrittivo, come qualsiasi errore che si è verificato durante la modifica o eliminazione di processo ha generato un'eccezione non gestita. Ad esempio, omettere il nome del prodotto s o quando si modifica un prodotto, immettere il valore di prezzo molto conveniente!, genera un'eccezione. Poiché nel codice non viene rilevata questa eccezione, propagata al runtime di ASP.NET, che consente di visualizzare i dettagli dell'eccezione s nella pagina web.

Come illustrato nel [BLL - gestione e le eccezioni DAL livello in una pagina ASP.NET](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) esercitazione, se viene generata un'eccezione dalle profondità della logica di Business o i livelli di accesso ai dati, i dettagli dell'eccezione vengono restituiti a ObjectDataSource e quindi a GridView. È stato illustrato come gestire correttamente queste eccezioni creando `Updated` o `RowUpdated` gestori eventi per il ObjectDataSource o GridView, verifica di un'eccezione e quindi che indica che è stata gestita l'eccezione.

Le DataList esercitazioni, tuttavia, t utilizzando ObjectDataSource per l'aggiornamento e l'eliminazione dei dati. In alternativa, stiamo lavorando direttamente in base al livello Business LOGIC. Per rilevare le eccezioni provenienti dal livello Business LOGIC o DAL, è necessario implementare il code-behind della nostra pagina ASP.NET all'interno di gestione delle eccezioni. In questa esercitazione, vedremo come più tactfully gestire le eccezioni generate durante una s DataList modificabile, l'aggiornamento del flusso di lavoro.

> [!NOTE]
> Nel *una panoramica di modifica e l'eliminazione di dati in DataList* esercitazione abbiamo parlato delle diverse tecniche per la modifica e l'eliminazione di dati da DataList alcune tecniche coinvolti utilizzando ObjectDataSource per l'aggiornamento e l'eliminazione. Se queste tecniche, è possibile gestire eccezioni dal BLL o DAL attraverso il s ObjectDataSource `Updated` o `Deleted` gestori eventi.


## <a name="step-1-creating-an-editable-datalist"></a>Passaggio 1: Creazione di un DataList modificabile

Prima di preoccupazione la gestione delle eccezioni che si verificano durante l'aggiornamento del flusso di lavoro, consentire s creare innanzitutto un DataList modificabile. Aprire il `ErrorHandling.aspx` nella pagina il `EditDeleteDataList` cartella, aggiungere un controllo DataList nella finestra di progettazione, imposta il relativo `ID` proprietà `Products`, e aggiungere un nuovo oggetto ObjectDataSource denominato `ProductsDataSource`. Configurare ObjectDataSource per utilizzare il `ProductsBLL` classe s `GetProducts()` metodo per la selezione di record, impostare gli elenchi a discesa nell'istruzione INSERT, UPDATE ed eliminare le tabulazioni su (nessuno).


[![Restituire le informazioni sul prodotto utilizzando il metodo GetProducts()](handling-bll-and-dal-level-exceptions-vb/_static/image2.png)](handling-bll-and-dal-level-exceptions-vb/_static/image1.png)

**Figura 1**: restituire le informazioni prodotto utilizzando il `GetProducts()` metodo ([fare clic per visualizzare l'immagine ingrandita](handling-bll-and-dal-level-exceptions-vb/_static/image3.png))


Dopo aver completato la procedura guidata ObjectDataSource, Visual Studio crea automaticamente un `ItemTemplate` per DataList. Sostituire con un `ItemTemplate` che consente di visualizzare ogni nome di prodotto s e prezzo e include un pulsante Modifica. Creare quindi un `EditItemTemplate` con un controllo casella di testo Web per nome, prezzo e pulsanti Aggiorna e Annulla. Infine, impostare DataList s `RepeatColumns` proprietà su 2.

Dopo aver apportato queste modifiche, markup dichiarativo s pagina dovrebbe essere simile al seguente. Controllare per verificare che la modifica, annullamento, e i pulsanti di aggiornamento, i relativi `CommandName` proprietà impostate per modificare, annullare e aggiornare, rispettivamente.


[!code-aspx[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample1.aspx)]

> [!NOTE]
> Per questa esercitazione DataList è necessario abilitare lo stato di visualizzazione s.


Dedicare alcuni minuti per visualizzare l'avanzamento attraverso un browser (vedere la figura 2).


[![Ogni prodotto include un pulsante Modifica](handling-bll-and-dal-level-exceptions-vb/_static/image5.png)](handling-bll-and-dal-level-exceptions-vb/_static/image4.png)

**Figura 2**: ogni prodotto include un pulsante Modifica ([fare clic per visualizzare l'immagine ingrandita](handling-bll-and-dal-level-exceptions-vb/_static/image6.png))


Attualmente, solo il pulsante modifica provoca un postback, t ancora rendono il prodotto modificabile. Per consentire la modifica, è necessario creare gestori eventi per il controllo DataList s `EditCommand`, `CancelCommand`, e `UpdateCommand` eventi. Il `EditCommand` e `CancelCommand` eventi limitarsi ad aggiornare DataList s `EditItemIndex` proprietà e i dati da DataList riassociazione:


[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample2.vb)]

Il `UpdateCommand` gestore eventi è un po' più complicata. È necessario leggere nel prodotto modificato s `ProductID` dal `DataKeys` insieme con il nome del prodotto s e il prezzo da caselle di testo nel `EditItemTemplate`e quindi chiamare il `ProductsBLL` classe s `UpdateProduct` prima della restituzione DataList (metodo) lo stato di pre-modifica.

Per il momento, s consentono di usare semplicemente esattamente lo stesso codice dal `UpdateCommand` gestore dell'evento nel *Panoramica della modifica e l'eliminazione di dati in DataList* esercitazione. Verrà aggiunto il codice per gestire correttamente le eccezioni nel passaggio 2.


[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample3.vb)]

In caso di input non valido che può essere sotto forma di un prezzo unitario formattato in modo errato, un valore del prezzo di unità non valida come - 5.00$ o l'omissione del nome del prodotto di s che verrà generata un'eccezione. Poiché il `UpdateCommand` gestore dell'evento non include eventuali eccezioni codice a questo punto, l'eccezione passerà al runtime di ASP.NET, in cui verrà visualizzato all'utente finale (vedere la figura 3).


![Quando si verifica un'eccezione non gestita, l'utente finale vedrà una pagina di errore](handling-bll-and-dal-level-exceptions-vb/_static/image7.png)

**Figura 3**: quando si verifica un'eccezione non gestita, l'utente finale vedrà una pagina di errore


## <a name="step-2-gracefully-handling-exceptions-in-the-updatecommand-event-handler"></a>Passaggio 2: Normalmente la gestione delle eccezioni nel gestore eventi UpdateCommand

Durante l'aggiornamento del flusso di lavoro, possono verificarsi eccezioni nel `UpdateCommand` DAL gestore eventi o il livello Business LOGIC. Ad esempio, se un utente immette un prezzo di troppi costosi, il `Decimal.Parse` istruzione il `UpdateCommand` gestore genererà un `FormatException` eccezione. Se si omette il nome del prodotto s o se il prezzo ha un valore negativo, DAL genererà un'eccezione.

Quando si verifica un'eccezione, si vuole visualizzare un messaggio informativo all'interno della pagina stessa. Aggiungere un'etichetta di Web controllo alla pagina di cui `ID` è impostato su `ExceptionDetails`. Configurare il testo dell'etichetta s per visualizzare in rosso, molto grande, tipo di carattere grassetto e corsivo assegnando il relativo `CssClass` proprietà il `Warning` classe CSS, che è definito nel `Styles.css` file.

Quando si verifica un errore, è necessario solo l'etichetta da visualizzare una sola volta. Vale a dire nei postback successivi, il messaggio di avviso etichetta s deve essere nascosto. Questo può essere eseguito cancellando entrambi l'etichetta s `Text` impostazioni o proprietà relativo `Visible` proprietà `False` nel `Page_Load` gestore dell'evento (come è stato fatto nel [BLL - gestione e le eccezioni DAL livello in una pagina ASP Pagina .NET](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md) esercitazione) o disabilitare il supporto dello stato di visualizzazione etichetta s. Consente di utilizzare la seconda opzione s.


[!code-aspx[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample4.aspx)]

Quando viene generata un'eccezione, si verranno assegnato i dettagli dell'eccezione per il `ExceptionDetails` s controllo etichetta `Text` proprietà. Poiché lo stato di visualizzazione è disabilitato, nei postback successivi il `Text` modifiche a livello di codice di proprietà s andranno perse, eseguendo il ripristino di testo predefinita (una stringa vuota), quindi nascondendo il messaggio di avviso.

Per determinare se è stato generato un errore per visualizzare un messaggio utile nella pagina, è necessario aggiungere un `Try ... Catch` blocco per il `UpdateCommand` gestore dell'evento. Il `Try` parte contiene codice che può causare un'eccezione, mentre il `Catch` blocco contiene il codice che viene eseguito in caso di un'eccezione. Estrarre il [nozioni fondamentali sulla gestione delle eccezioni](https://msdn.microsoft.com/library/2w8f0bss.aspx) sezione nella documentazione di .NET Framework per altre informazioni di `Try ... Catch` blocco.


[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample5.vb)]

Quando viene generata un'eccezione di qualsiasi tipo dal codice all'interno di `Try` blocco, il `Catch` codice blocco s verrà avviata l'esecuzione. Il tipo di eccezione che viene generata un'eccezione `DbException`, `NoNullAllowedException`, `ArgumentException`e così via dipende da quali, esattamente, comportato la di in primo luogo l'errore. Se esiste un problema s a livello di database, un `DbException` verrà generata. Se viene immesso un valore non valido per il `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, o `ReorderLevel` campi, un `ArgumentException` verrà generata, come è stato aggiunto il codice per convalidare i valori di campo nel `ProductsDataTable` classe (vedere il [ Creazione di un livello di logica di Business](../introduction/creating-a-business-logic-layer-vb.md) esercitazione).

Possiamo fornire una spiegazione più utile all'utente finale basando il testo del messaggio del tipo di eccezione rilevata. Il codice seguente che è stato utilizzato in un form quasi identico nel [BLL - gestione e le eccezioni DAL livello in una pagina ASP.NET](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md) esercitazione fornisce questo livello di dettaglio:


[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample6.vb)]

Per completare questa esercitazione, chiamare semplicemente il `DisplayExceptionDetails` metodo il `Catch` blocco passando intercettato `Exception` istanza (`ex`).

Con il `Try ... Catch` bloccare sul posto, gli utenti vengono presentati con un messaggio di errore più descrittivo, come nelle figure 4 e 5 Mostra. Si noti che in caso di un'eccezione DataList rimangono in modalità di modifica. Infatti, quando si verifica l'eccezione, il flusso di controllo viene immediatamente reindirizzato per il `Catch` blocco, ignorando il codice che restituisce lo stato di pre-modifica DataList.


[![Viene visualizzato un messaggio di errore se si omette un campo obbligatorio](handling-bll-and-dal-level-exceptions-vb/_static/image9.png)](handling-bll-and-dal-level-exceptions-vb/_static/image8.png)

**Figura 4**: viene visualizzato un messaggio di errore se si omette un campo obbligatorio ([fare clic per visualizzare l'immagine ingrandita](handling-bll-and-dal-level-exceptions-vb/_static/image10.png))


[![Un messaggio di errore viene visualizzato quando immettendo un prezzo negativo](handling-bll-and-dal-level-exceptions-vb/_static/image12.png)](handling-bll-and-dal-level-exceptions-vb/_static/image11.png)

**Figura 5**: un messaggio di errore viene visualizzato quando immettendo un prezzo negativo ([fare clic per visualizzare l'immagine ingrandita](handling-bll-and-dal-level-exceptions-vb/_static/image13.png))


## <a name="summary"></a>Riepilogo

GridView e ObjectDataSource fornire gestori di eventi di post-livello che includono informazioni su tutte le eccezioni generate durante l'aggiornamento e l'eliminazione del flusso di lavoro, nonché le proprietà che è possibile impostare per indicare se l'eccezione è stata gestito. Queste funzionalità, tuttavia, non sono disponibili quando si con DataList e si utilizza il livello Business LOGIC direttamente. In alternativa, è sono responsabili dell'implementazione di gestione delle eccezioni.

In questa esercitazione è stato illustrato come aggiungere eccezioni per una s DataList modificabile, l'aggiornamento del flusso di lavoro aggiungendo un `Try ... Catch` blocco per il `UpdateCommand` gestore dell'evento. Se viene generata un'eccezione durante l'aggiornamento del flusso di lavoro, il `Catch` codice blocco s viene eseguita, la visualizzazione di informazioni utili nel `ExceptionDetails` etichetta.

A questo punto, DataList non vengono effettuate operazioni per evitare le eccezioni che si verifichino in primo luogo. Anche se si sa che un prezzo negativo genererà un'eccezione, sono utili t ancora aggiunto alcuna funzionalità per evitare in modo proattivo che un utente l'immissione di input di questo tipo non valido. Nell'esercitazione successiva vedremo come ridurre le eccezioni causate da input dell'utente non valido tramite l'aggiunta di controlli di convalida nel `EditItemTemplate`.

Buona programmazione!

## <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Linee guida di progettazione delle eccezioni](https://msdn.microsoft.com/library/ms298399.aspx)
- [Moduli di registrazione errore e i gestori (ELMAH)](http://workspaces.gotdotnet.com/elmah) (una libreria open source per la registrazione degli errori)
- [Enterprise Library per .NET Framework 2.0](https://www.microsoft.com/downloads/details.aspx?familyid=5A14E870-406B-4F2A-B723-97BA84AE80B5&amp;displaylang=en) (include il blocco applicativo di gestione eccezioni)

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore per questa esercitazione è stata Ken Pespisa. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](performing-batch-updates-vb.md)
> [Successivo](adding-validation-controls-to-the-datalist-s-editing-interface-vb.md)
