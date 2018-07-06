---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/handling-bll-and-dal-level-exceptions-vb
title: Gestione delle eccezioni a livello BLL e DAL (VB) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione, vedremo come tactfully gestire le eccezioni generate durante l'aggiornamento flusso di lavoro di DataList un modificabile.
ms.author: aspnetcontent
ms.date: 10/30/2006
ms.assetid: ca665073-b379-4239-9404-f597663ca65e
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/handling-bll-and-dal-level-exceptions-vb
msc.type: authoredcontent
ms.openlocfilehash: 1aee5c26bfbbbc46c2151a56fed60057930cba80
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814252"
---
<a name="handling-bll--and-dal-level-exceptions-vb"></a>Gestione delle eccezioni a livello BLL e DAL (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'App di esempio](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_38_VB.exe) o [Scarica il PDF](handling-bll-and-dal-level-exceptions-vb/_static/datatutorial38vb1.pdf)

> In questa esercitazione, vedremo come tactfully gestire le eccezioni generate durante l'aggiornamento flusso di lavoro di DataList un modificabile.


## <a name="introduction"></a>Introduzione

Nel [Panoramica di modifica e l'eliminazione dei dati in DataList](an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md) esercitazione, è stato creato un controllo DataList che offerte semplice eliminazione e modifica di funzionalità. Anche se completamente funzionante, era praticamente descrittivo, come qualsiasi errore verificatosi durante il processo di modifica o eliminazione di processo ha generato un'eccezione non gestita. Ad esempio, omettendo il nome del prodotto s o, quando si modifica un prodotto, immettendo un valore del prezzo di molto conveniente!, genera un'eccezione. Poiché questa eccezione non rilevata nel codice, propagata fino al runtime di ASP.NET, che consente di visualizzare i dettagli dell'eccezione s nella pagina web.

Come abbiamo visto nel [BLL - la gestione e le eccezioni di livello in una pagina ASP.NET](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) esercitazione, se viene generata un'eccezione dalle profondità della logica di Business o livelli di accesso ai dati, i dettagli dell'eccezione vengono restituiti da ObjectDataSource e quindi a GridView. È stato illustrato come gestire queste eccezioni correttamente creando `Updated` o `RowUpdated` gestori eventi per l'oggetto ObjectDataSource o GridView, cercando un'eccezione e quindi che indica che è stata gestita l'eccezione.

DataList esercitazioni, tuttavia, t utilizzando ObjectDataSource per l'aggiornamento ed eliminazione dei dati. Al contrario, Microsoft sta lavorando direttamente con il livello BLL. Per rilevare le eccezioni che provengono dal livello BLL o DAL, è necessario implementare codice all'interno di code-behind della nostra pagina ASP.NET di gestione delle eccezioni. In questa esercitazione, vedremo in che modo gestire più tactfully le eccezioni generate durante una s DataList modificabile l'aggiornamento del flusso di lavoro.

> [!NOTE]
> Nel *una panoramica di modifica e l'eliminazione dei dati in DataList* esercitazione sono illustrate le diverse tecniche per la modifica ed eliminazione dei dati da DataList, alcune tecniche coinvolte utilizzando ObjectDataSource per l'aggiornamento e l'eliminazione. Se si utilizzano queste tecniche, è possibile gestire le eccezioni dal livello BLL o DAL tramite gli oggetti ObjectDataSource `Updated` o `Deleted` gestori eventi.


## <a name="step-1-creating-an-editable-datalist"></a>Passaggio 1: Creazione di un DataList modificabile

Prima ci preoccupiamo la gestione delle eccezioni che si verificano durante l'aggiornamento del flusso di lavoro, consentire s prima di tutto creare un DataList modificabile. Aprire il `ErrorHandling.aspx` nella pagina la `EditDeleteDataList` cartella, aggiungere un controllo DataList alla finestra di progettazione, imposta relativo `ID` proprietà `Products`, e aggiungere un nuovo oggetto ObjectDataSource denominato `ProductsDataSource`. Configurare ObjectDataSource per usare la `ProductsBLL` classe s `GetProducts()` metodo per la selezione di registra; impostare gli elenchi a discesa nell'istruzione INSERT, UPDATE ed eliminare schede su (nessuno).


[![Restituisce le informazioni sul prodotto utilizzando il metodo GetProducts()](handling-bll-and-dal-level-exceptions-vb/_static/image2.png)](handling-bll-and-dal-level-exceptions-vb/_static/image1.png)

**Figura 1**: restituisce le informazioni di prodotto usando il `GetProducts()` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](handling-bll-and-dal-level-exceptions-vb/_static/image3.png))


Dopo aver completato la procedura guidata ObjectDataSource, Visual Studio creerà automaticamente un `ItemTemplate` per DataList. Sostituire con un `ItemTemplate` che visualizza ogni nome di prodotto s e il prezzo e include un pulsante di modifica. Creare quindi un `EditItemTemplate` con un controllo TextBox Web per nome e prezzo e i pulsanti Annulla e Update. Infine, impostare il controllo DataList s `RepeatColumns` proprietà su 2.

Dopo tali modifiche, il markup dichiarativo s pagina dovrebbe essere simile al seguente. Controllare per verificare che la modifica, Cancel, e pulsanti di aggiornamento è stato assegnato loro `CommandName` impostate su Modifica, annullare e aggiornare, rispettivamente.


[!code-aspx[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample1.aspx)]

> [!NOTE]
> Per questa esercitazione DataList lo stato di visualizzazione s deve essere abilitato.


Si consiglia di visualizzare lo stato di avanzamento tramite un browser (vedere la figura 2).


[![Ogni prodotto include un pulsante Modifica](handling-bll-and-dal-level-exceptions-vb/_static/image5.png)](handling-bll-and-dal-level-exceptions-vb/_static/image4.png)

**Figura 2**: ogni prodotto include un pulsante Modifica ([fare clic per visualizzare l'immagine con dimensioni normali](handling-bll-and-dal-level-exceptions-vb/_static/image6.png))


Attualmente, il pulsante Modifica fa sì che solo un postback, t l ancora consente di rendere il prodotto modificabile. Per abilitare la modifica, è necessario creare i gestori eventi per s DataList `EditCommand`, `CancelCommand`, e `UpdateCommand` eventi. Il `EditCommand` e `CancelCommand` eventi aggiornare semplicemente DataList s `EditItemIndex` proprietà e i dati per il controllo DataList riassociazione:


[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample2.vb)]

Il `UpdateCommand` gestore eventi è un po' più complessa. Per la lettura nel prodotto modificato s `ProductID` dal `DataKeys` insieme con il nome del prodotto s e il prezzo da caselle di testo nel `EditItemTemplate`, quindi chiamare il `ProductsBLL` classe s `UpdateProduct` prima della restituzione di DataList (metodo) lo stato di pre-editing.

Per ora, ti permettono di s usare semplicemente esattamente lo stesso codice dal `UpdateCommand` gestore dell'evento nel *Panoramica di modifica e l'eliminazione dei dati in DataList* esercitazione. Si aggiungerà il codice per gestire correttamente le eccezioni nel passaggio 2.


[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample3.vb)]

In caso di input non valido che può essere sotto forma di un prezzo unitario formattato in modo errato, un valore del prezzo unità non valida, ad esempio - 5,00 dollari USA o l'omissione del nome del prodotto di s che verrà generata un'eccezione. Poiché il `UpdateCommand` gestore dell'evento non include alcun codice gestione delle eccezioni a questo punto, l'eccezione esegue il bubbling fino al runtime di ASP.NET, in cui verrà visualizzato all'utente finale (vedere la figura 3).


![Quando si verifica un'eccezione non gestita, l'utente finale vedrà una pagina di errore](handling-bll-and-dal-level-exceptions-vb/_static/image7.png)

**Figura 3**: quando si verifica un'eccezione non gestita, l'utente finale vedrà una pagina di errore


## <a name="step-2-gracefully-handling-exceptions-in-the-updatecommand-event-handler"></a>Passaggio 2: Normalmente la gestione delle eccezioni nel gestore dell'evento UpdateCommand

Durante l'aggiornamento del flusso di lavoro, le eccezioni possano verificarsi nel `UpdateCommand` gestore dell'evento, il livello BLL e DAL. Ad esempio, se un utente immette un prezzo di troppi costosi, la `Decimal.Parse` istruzione nel `UpdateCommand` genera gestore eventi una `FormatException` eccezione. Se l'utente la omette il nome del prodotto s o se il prezzo ha un valore negativo, il campo DAL genererà un'eccezione.

Quando si verifica un'eccezione, si vuole visualizzare un messaggio informativo all'interno della pagina stessa. Aggiungere un'etichetta Web controllo alla pagina di cui `ID` è impostata su `ExceptionDetails`. Configurare il testo dell'etichetta s per visualizzare in rosso, molto grande, del carattere grassetto e corsivo assegnando relativi `CssClass` proprietà per il `Warning` classe CSS che è definito nel `Styles.css` file.

Quando si verifica un errore, si vuole solo che l'etichetta da visualizzare una sola volta. Vale a dire, durante i postback successivi, il messaggio di avviso etichetta s scomparirà. A questo scopo cancellando entrambi l'etichetta s `Text` impostazioni o proprietà relativi `Visible` proprietà `False` nel `Page_Load` gestore dell'evento (come abbiamo fatto nel [BLL - la gestione e le eccezioni di livello in una pagina ASP Pagina .NET](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md) esercitazione) oppure disabilitare il supporto dello stato di visualizzazione etichetta s. Consentire s utilizzare l'opzione di quest'ultima.


[!code-aspx[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample4.aspx)]

Quando viene generata un'eccezione, si assegneranno i dettagli dell'eccezione per il `ExceptionDetails` controllo s etichetta `Text` proprietà. Poiché lo stato di visualizzazione è disabilitato, durante i postback successivi il `Text` le modifiche a livello di codice alle proprietà s andranno persi, eseguendo il ripristino del testo predefinito (una stringa vuota), quindi nascondendo il messaggio di avviso.

Per determinare quando è stato generato un errore per visualizzare un messaggio utile nella pagina, è necessario aggiungere un `Try ... Catch` bloccare il `UpdateCommand` gestore dell'evento. Il `Try` parte contiene codice che può causare un'eccezione, mentre il `Catch` blocco contiene codice che viene eseguito in caso di un'eccezione. Consultare il [nozioni fondamentali sulla gestione delle eccezioni](https://msdn.microsoft.com/library/2w8f0bss.aspx) sezione nella documentazione di .NET Framework per altre informazioni sul `Try ... Catch` blocco.


[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample5.vb)]

Quando viene generata un'eccezione di qualsiasi tipo dal codice all'interno di `Try` blocco, il `Catch` blocco s codice avvia l'esecuzione. Il tipo di eccezione che viene generata un'eccezione `DbException`, `NoNullAllowedException`, `ArgumentException`e così via dipende da ciò che, esattamente, comportato in primo luogo l'errore. Se tale posizione s un problema a livello di database, un `DbException` verrà generata. Se viene immesso un valore non valido per il `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, o `ReorderLevel` campi, una `ArgumentException` verrà generata, come è stato aggiunto codice per convalidare i valori dei campi nel `ProductsDataTable` classe (vedere il [ Creazione di un livello di logica di Business](../introduction/creating-a-business-logic-layer-vb.md) esercitazione).

Possiamo fornire una spiegazione più utile all'utente finale da basare il testo del messaggio del tipo di eccezione rilevata. Il codice seguente che è stato usato in un form quasi identico nel [BLL - la gestione e le eccezioni di livello in una pagina ASP.NET](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md) esercitazione fornisce questo livello di dettaglio:


[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample6.vb)]

Per completare questa esercitazione, è sufficiente chiamare il `DisplayExceptionDetails` metodo dal `Catch` blocco passando intercettato `Exception` istanza (`ex`).

Con la `Try ... Catch` blocco posto, gli utenti visualizzano un messaggio di errore più informativo, come nelle figure 4 e 5 show. Si noti che in caso di un'eccezione di DataList rimangano in modalità di modifica. Infatti, quando si verifica l'eccezione, il flusso di controllo viene immediatamente reindirizzato alla pagina di `Catch` blocco, ignorando il codice che ripristina lo stato di pre-modifica di DataList.


[![Viene visualizzato un messaggio di errore quando un utente la omette un campo obbligatorio](handling-bll-and-dal-level-exceptions-vb/_static/image9.png)](handling-bll-and-dal-level-exceptions-vb/_static/image8.png)

**Figura 4**: viene visualizzato un messaggio di errore quando un utente la omette un campo obbligatorio ([fare clic per visualizzare l'immagine con dimensioni normali](handling-bll-and-dal-level-exceptions-vb/_static/image10.png))


[![Un messaggio di errore viene visualizzato quando immettendo un prezzo negativo](handling-bll-and-dal-level-exceptions-vb/_static/image12.png)](handling-bll-and-dal-level-exceptions-vb/_static/image11.png)

**Figura 5**: un messaggio di errore viene visualizzato quando immettendo un prezzo negativo ([fare clic per visualizzare l'immagine con dimensioni normali](handling-bll-and-dal-level-exceptions-vb/_static/image13.png))


## <a name="summary"></a>Riepilogo

Il controllo GridView e ObjectDataSource fornire gestori di evento di post-livello che includono informazioni sulle eventuali eccezioni che sono state generate durante l'aggiornamento e l'eliminazione del flusso di lavoro, nonché le proprietà che è possibile impostare per indicare se l'eccezione è stata gestita. Queste funzionalità, tuttavia, non sono disponibili quando si con DataList e si utilizza direttamente il livello BLL. Al contrario, siamo tenuti a implementare la gestione delle eccezioni.

In questa esercitazione è stato illustrato come aggiungere una s DataList modificabile l'aggiornamento del flusso di lavoro aggiungendo la gestione delle eccezioni una `Try ... Catch` bloccare il `UpdateCommand` gestore dell'evento. Se viene generata un'eccezione durante l'aggiornamento del flusso di lavoro, il `Catch` viene eseguito il codice di blocco s, la visualizzazione di informazioni utili nella `ExceptionDetails` etichetta.

A questo punto, il controllo DataList in alcun modo per evitare eccezioni che si verifichino problemi in primo luogo. Anche se sappiamo che un prezzo negativo genererà un'eccezione, è che t è ancora aggiunto alcuna funzionalità per impedire in modo proattivo un utente di immettere input di questo tipo non valido. Nell'esercitazione successiva si vedrà come limitare il eccezioni causate da input utente non valido aggiungendo i controlli di convalida nel `EditItemTemplate`.

Buona programmazione!

## <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Linee guida di progettazione delle eccezioni](https://msdn.microsoft.com/library/ms298399.aspx)
- [Error Logging Modules e gestori (ELMAH)](http://workspaces.gotdotnet.com/elmah) (una libreria open source per la registrazione degli errori)
- [Enterprise Library per .NET Framework 2.0](https://www.microsoft.com/downloads/details.aspx?familyid=5A14E870-406B-4F2A-B723-97BA84AE80B5&amp;displaylang=en) (include Exception Management Application Block)

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore capo di questa esercitazione è stata Ken Pespisa. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](performing-batch-updates-vb.md)
> [Successivo](adding-validation-controls-to-the-datalist-s-editing-interface-vb.md)
