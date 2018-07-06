---
uid: web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-content-page-from-the-master-page-cs
title: L'interazione con la pagina di contenuto dalla pagina Master (c#) | Microsoft Docs
author: rick-anderson
description: Viene illustrato come chiamare i metodi, impostare le proprietà e così via della pagina contenuto dal codice nella pagina Master.
ms.author: aspnetcontent
ms.date: 07/11/2008
ms.assetid: 3282df5e-516c-4972-8666-313828b90fb5
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-content-page-from-the-master-page-cs
msc.type: authoredcontent
ms.openlocfilehash: 293e4dab6142393c9d57836a2f04244388e54cec
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808366"
---
<a name="interacting-with-the-content-page-from-the-master-page-c"></a>L'interazione con la pagina di contenuto dalla pagina Master (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare il codice](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_07_CS.zip) o [Scarica il PDF](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_07_CS.pdf)

> Viene illustrato come chiamare i metodi, impostare le proprietà e così via della pagina contenuto dal codice nella pagina Master.


## <a name="introduction"></a>Introduzione

L'esercitazione precedente esaminato il modo avere la pagina di contenuto a livello di programmazione interagire con la pagina master. Tenere presente che abbiamo aggiornato la pagina master per includere un controllo GridView elencato i cinque più di recente aggiunta prodotti. Viene quindi creata una pagina di contenuto da cui l'utente è stato possibile aggiungere un nuovo prodotto. Quando viene aggiunta a un nuovo prodotto, la pagina di contenuto necessario per indicare la pagina master per aggiornare il controllo GridView in modo che si includa il prodotto appena aggiunti. Questa funzionalità è stata effettuata aggiungendo un metodo pubblico della pagina master aggiornato con associazione a dati a GridView e quindi richiamare tale metodo dalla pagina contenuto.

La forma più comune di contenuto e l'interazione di pagina master ha origine dalla pagina contenuto. Tuttavia, è possibile che la pagina master per rouse la pagina di contenuto corrente in azioni e questa funzionalità potrebbe essere necessario se la pagina master contiene elementi dell'interfaccia utente che consentono agli utenti di modificare i dati che viene visualizzati anche nella pagina contenuto. Si consideri una pagina di contenuto che consente di visualizzare le informazioni sui prodotti in un controllo GridView di controllare e una pagina master che include un pulsante che, quando si fa clic, raddoppia i prezzi di tutti i prodotti. Molto simile a quello nell'esercitazione precedente, il controllo GridView deve essere aggiornato dopo il prezzo doppio che è fatto clic sul pulsante in modo da visualizzare i nuovi prezzi, ma in questo scenario è la pagina master che è necessario rouse pagina contenuto in azione.

Questa esercitazione illustra come impostare la pagina master richiamare funzionalità definite nella pagina di contenuto.

### <a name="instigating-programmatic-interaction-via-an-event-and-event-handlers"></a>Instigating interazione a livello di codice tramite un evento e i gestori eventi

Richiamare la funzionalità di pagina di contenuto da una pagina master è tanto impegnativa quanto nel caso opposto. Perché una pagina di contenuto ha una singola pagina master, quando l'interazione a livello di codice dalla pagina contenuto instigating Sappiamo cosa metodi pubblici e le proprietà sono a disposizione. Una pagina master, tuttavia, può avere numerose pagine di contenuto diversi, ognuno con un proprio set di proprietà e metodi. Quindi, la scrittura di codice nella pagina master per eseguire un'azione nella relativa pagina di contenuto quando non si sa quale pagina contenuto verrà richiamato fino al runtime?

Si consideri un controllo Web ASP.NET, ad esempio il controllo pulsante. Un controllo pulsante può essere visualizzati in un numero qualsiasi di pagine ASP.NET e richiede un meccanismo mediante il quale è possibile verificare la pagina che si è stato fatto clic. Ciò avviene utilizzando *eventi*. In particolare, il controllo pulsante genera relativi `Click` evento quando viene selezionato; la pagina ASP.NET che contiene il pulsante può facoltativamente rispondere al notifica tramite un *gestore dell'evento*.

Questo stesso modello può essere usato per avere una funzionalità di trigger della pagina master nelle relative pagine di contenuto:

1. Aggiungere un evento della pagina master.
2. Generare l'evento ogni volta che la pagina master deve poter comunicare con relativa pagina di contenuto. Ad esempio, se la pagina master deve essere di avviso relativa pagina di contenuto che l'utente è raddoppiato i prezzi, il relativo evento potrebbe essere generato immediatamente dopo i prezzi indicati sono stati raddoppiati.
3. Creare un gestore eventi in tali pagine di contenuto che è necessario eseguire alcune operazioni.

La parte restante di questa esercitazione implementa l'esempio illustrato nella sezione introduttiva; vale a dire, una pagina di contenuto in cui sono elencati i prodotti nel database e una pagina master che include un pulsante di controllo per raddoppiare i prezzi.

## <a name="step-1-displaying-products-in-a-content-page"></a>Passaggio 1: Visualizzazione dei prodotti in una pagina di contenuto

Il primo è creare una pagina di contenuto in cui sono elencati i prodotti dal database Northwind. (Il database Northwind è stato aggiunto al progetto nell'esercitazione precedente [ *che interagiscono con la pagina Master dalla pagina contenuto*](interacting-with-the-master-page-from-the-content-page-cs.md).) Iniziare aggiungendo una nuova pagina ASP.NET per la `~/Admin` cartella denominata `Products.aspx`, assicurandosi di associarlo al `Site.master` pagina master. Figura 1 Mostra Esplora soluzioni dopo l'aggiunta di questa pagina per il sito Web.


[![Aggiungere una nuova pagina di ASP.NET per la cartella Admin](interacting-with-the-content-page-from-the-master-page-cs/_static/image2.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image1.png)

**Figura 01**: aggiungere una nuova pagina ASP.NET per il `Admin` cartella ([fare clic per visualizzare l'immagine con dimensioni normali](interacting-with-the-content-page-from-the-master-page-cs/_static/image3.png))


Si tenga presente che nel [ *specificazione di titolo, metatag e altre intestazioni HTML nella pagina Master* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) esercitazione è stata creata una classe di pagina base personalizzata denominata `BasePage` che genera il titolo della pagina, se non è impostare in modo esplicito. Andare alla `Products.aspx` code-behind della pagina classe e fare in modo che derivano da `BasePage` (anziché da `System.Web.UI.Page`).

Aggiornare infine il `Web.sitemap` file da includere una voce per questa lezione. Aggiungere il markup seguente sotto il `<siteMapNode>` per il contenuto alla lezione l'interazione di pagina Master:


[!code-xml[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample1.xml)]

L'aggiunta di questo `<siteMapNode>` elemento si riflette nelle lezioni dell'elenco (vedere la figura 5).

Tornare al `Products.aspx`. Nel controllo del contenuto per `MainContent`, aggiungere un controllo GridView e denominarlo `ProductsGrid`. Associare il controllo GridView per un nuovo controllo SqlDataSource denominato `ProductsDataSource`.


[![Associare il controllo GridView per un nuovo controllo SqlDataSource](interacting-with-the-content-page-from-the-master-page-cs/_static/image5.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image4.png)

**Figura 02**: associa il controllo GridView per un nuovo controllo SqlDataSource ([fare clic per visualizzare l'immagine con dimensioni normali](interacting-with-the-content-page-from-the-master-page-cs/_static/image6.png))


Configurare la procedura guidata in modo che utilizzi il database Northwind. Se si è lavorato l'esercitazione precedente, è necessario avere una stringa di connessione denominata `NorthwindConnectionString` in `Web.config`. Scegliere questa stringa di connessione tra l'elenco a discesa, come illustrato nella figura 3.


[![Configurare SqlDataSource per usare il Database Northwind](interacting-with-the-content-page-from-the-master-page-cs/_static/image8.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image7.png)

**Figura 03**: configurare SqlDataSource per usare il Northwind Database ([fare clic per visualizzare l'immagine con dimensioni normali](interacting-with-the-content-page-from-the-master-page-cs/_static/image9.png))


Specificare quindi il controllo origine dati `SELECT` istruzione scegliendo la tabella Products dall'elenco a discesa e restituendo il `ProductName` e `UnitPrice` colonne (vedere la figura 4). Fare clic su Avanti e quindi scegliere Fine per completare la procedura guidata Configura origine dati.


[![Restituire i campi di UnitPrice e ProductName dalla tabella Products](interacting-with-the-content-page-from-the-master-page-cs/_static/image11.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image10.png)

**Figura 04**: restituisce il `ProductName` e `UnitPrice` i campi dal `Products` tabella ([fare clic per visualizzare l'immagine con dimensioni normali](interacting-with-the-content-page-from-the-master-page-cs/_static/image12.png))


Questo è tutto qui! Dopo aver completato la procedura guidata Visual Studio aggiunge due BoundField di GridView per rispecchiare i due campi restituiti dal controllo SqlDataSource. Markup dei controlli GridView e SqlDataSource segue. Figura 5 mostra i risultati quando viene visualizzato tramite un browser.


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample2.aspx)]


[![Ogni prodotto e il prezzo è elencato in GridView](interacting-with-the-content-page-from-the-master-page-cs/_static/image14.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image13.png)

**Figura 05**: ogni prodotto e il prezzo è elencato in GridView ([fare clic per visualizzare l'immagine con dimensioni normali](interacting-with-the-content-page-from-the-master-page-cs/_static/image15.png))


> [!NOTE]
> È possibile pulire l'aspetto di GridView. Alcuni suggerimenti includono il valore di UnitPrice visualizzato come valuta di formattazione e l'utilizzo di tipi di carattere e colori di sfondo per migliorare l'aspetto della griglia. Per altre informazioni sulla visualizzazione e la formattazione dei dati in ASP.NET, fare riferimento a mio [utilizzo di serie di esercitazioni dati](../../data-access/index.md).


## <a name="step-2-adding-a-double-prices-button-to-the-master-page"></a>Passaggio 2: Aggiunta di un pulsante Double i prezzi per la pagina Master

L'attività successiva è aggiungere un controllo pulsante Web al server master di pagina. quando si fa clic, verrà raddoppiare il prezzo di tutti i prodotti nel database. Aprire il `Site.master` pagina master e trascinare un pulsante dalla casella degli strumenti nella finestra di progettazione, posizionandolo sotto il `RecentProductsDataSource` controllo SqlDataSource è stato aggiunto nell'esercitazione precedente. Impostare il pulsante `ID` proprietà `DoublePrice` e il relativo `Text` proprietà su "Double prezzi dei prodotti".

Successivamente, aggiungere un controllo SqlDataSource per la pagina master, denominarlo `DoublePricesDataSource`. Questo SqlDataSource verrà usato per eseguire il `UPDATE` istruzione a raddoppiare i prezzi indicati. In particolare, è necessario impostare relativi `ConnectionString` e `UpdateCommand` delle proprietà per la stringa di connessione appropriate e `UPDATE` istruzione. Quindi è necessario chiamare questo controllo SqlDataSource `Update` metodo quando la `DoublePrice` si fa clic sul pulsante. Per impostare il `ConnectionString` e `UpdateCommand` proprietà, selezionare il controllo SqlDataSource e quindi passare alla finestra Proprietà. Il `ConnectionString` elenchi di proprietà di tali stringhe di connessione già archiviate in `Web.config` in un elenco di riepilogo a discesa, scegliere il `NorthwindConnectionString` opzione come illustrato nella figura 6.


[![Configurare SqlDataSource per l'uso di NorthwindConnectionString](interacting-with-the-content-page-from-the-master-page-cs/_static/image17.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image16.png)

**Figura 06**: configurare SqlDataSource per usare il `NorthwindConnectionString` ([fare clic per visualizzare l'immagine con dimensioni normali](interacting-with-the-content-page-from-the-master-page-cs/_static/image18.png))


Per impostare il `UpdateCommand` proprietà, individuare l'opzione UpdateQuery nella finestra Proprietà. Questa proprietà, quando selezionata, viene visualizzato un pulsante con puntini di sospensione; Fare clic su questo pulsante per visualizzare la finestra di dialogo Editor comandi e parametri illustrata nella figura 7. Digitare il comando seguente `UPDATE` istruzione nella casella di testo della finestra di dialogo:


[!code-sql[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample3.sql)]

Destinato a raddoppiare di questa istruzione, quando eseguita, il `UnitPrice` valore per ogni record di `Products` tabella.


[![Impostare proprietà UpdateCommand del SqlDataSource](interacting-with-the-content-page-from-the-master-page-cs/_static/image20.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image19.png)

**Figura 07**: impostare SqlDataSource `UpdateCommand` proprietà ([fare clic per visualizzare l'immagine con dimensioni normali](interacting-with-the-content-page-from-the-master-page-cs/_static/image21.png))


Dopo l'impostazione di queste proprietà, markup dichiarativo i controlli pulsante e SqlDataSource dovrebbe essere simile al seguente:


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample4.aspx)]

Tutto resta che chiamare relativi `Update` metodo quando la `DoublePrice` si fa clic sul pulsante. Creare un `Click` gestore dell'evento per il `DoublePrice` pulsante e aggiungere il codice seguente:


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample5.cs)]

Per testare questa funzionalità, visitare il `~/Admin/Products.aspx` pagina abbiamo creato nel passaggio 1 e fare clic sul pulsante "Double prezzi dei prodotti". Fare clic sul pulsante causa un postback ed esegue la `DoublePrice` del pulsante `Click` gestore eventi, raddoppiare i prezzi di tutti i prodotti. Rendering della pagina, quindi nuovamente il markup viene restituito e visualizzato nuovamente nel browser. Il controllo GridView nella pagina di contenuto, Elenca, tuttavia, gli stessi prezzi prima di "Double prodotto prezzi" è stato fatto clic sul pulsante. Questo avviene perché i dati inizialmente caricati in GridView era stato archiviato nello stato di visualizzazione, in modo che non viene ricaricata durante i postback se non diversamente specificato. Se visita una pagina diversa, quindi tornare al `~/Admin/Products.aspx` pagina si noterà i prezzi aggiornati.

## <a name="step-3-raising-an-event-when-the-prices-are-doubled"></a>Passaggio 3: Generare un evento quando il prezzi sono raddoppiate

Poiché il controllo GridView nel `~/Admin/Products.aspx` pagina non si riflette immediatamente il raddoppiamento price, un utente comprensibilmente potrebbe pensare che fanno non è stata clic sul pulsante "Prezzi dei prodotti Double", o non funziona. Potrebbe provano facendo clic sul pulsante alcune altre volte, raddoppiare i prezzi più volte. Per risolvere il problema è necessario disporre della griglia del contenuto pagina visualizzati i nuovi prezzi subito dopo che sono raddoppiate.

Come descritto in precedenza in questa esercitazione, è necessario generare un evento nella pagina master, ogni volta che l'utente fa clic di `DoublePrice` pulsante. Un evento è un modo per una classe (un autore di eventi) per notificare a un altro un set di altre classi (i sottoscrittori di eventi) che si è verificato qualcosa di interessante. In questo esempio, la pagina master è l'autore di eventi; quelle il contenuto di pagine che interessano quando il `DoublePrice` si fa clic sul pulsante sono i sottoscrittori.

Una classe sottoscrive un evento tramite la creazione di un' *gestore dell'evento*, ovvero un metodo che viene eseguito in risposta all'evento generato. Il server di pubblicazione definisce gli eventi che egli genera definendo un *delegato dell'evento*. Il delegato dell'evento specifica che il gestore eventi deve accettare i parametri di input. In .NET Framework, i delegati non restituisce alcun valore e accetta due parametri di input:

- Un `Object`, che identifica l'origine evento, e
- Una classe derivata da `System.EventArgs`

Il secondo parametro passato a un gestore eventi può includere informazioni aggiuntive sull'evento. Mentre la base `EventArgs` classe non passare le informazioni di qualsiasi, .NET Framework include una serie di classi che estendono `EventArgs` e comprendono proprietà aggiuntive. Ad esempio, un `CommandEventArgs` istanza viene passata ai gestori di eventi che rispondono al `Command` evento e include due proprietà informativo: `CommandArgument` e `CommandName`.

> [!NOTE]
> Per altre informazioni sulla creazione, la generazione e la gestione degli eventi, vedere [eventi e delegati](https://msdn.microsoft.com/library/17sde2xt.aspx) e [delegati di evento semplice inglese](http://www.codeproject.com/KB/cs/eventdelegates.aspx).


Per definire un evento di usare la sintassi seguente:


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample6.cs)]

Poiché è sufficiente ricevere un avviso nella pagina di contenuto quando l'utente ha fatto clic la `DoublePrice` pulsante e non è necessario passare a qualsiasi altra informazione aggiuntiva, è possibile usare il delegato dell'evento `EventHandler`, che definisce un gestore eventi che accetta come il secondo parametro di un oggetto di tipo `System.EventArgs`. Per creare l'evento nella pagina master, aggiungere la riga di codice seguente alla classe code-behind della pagina master:


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample7.cs)]

Il codice precedente aggiunge un evento pubblico della pagina master denominato `PricesDoubled`. È ora necessario generare l'evento dopo che i prezzi indicati sono stati raddoppiati. Per generare un evento di usare la sintassi seguente:


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample8.cs)]

In cui *mittente* e *eventArgs* sono i valori da passare al gestore eventi del sottoscrittore.

Aggiorna il `DoublePrice` `Click` gestore dell'evento con il codice seguente:


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample9.cs)]

Come in precedenza, il `Click` gestore dell'evento inizia chiamando le `DoublePricesDataSource` del controllo SqlDataSource `Update` metodo raddoppiare i prezzi di tutti i prodotti. I seguenti sono presenti due aggiunte al gestore dell'evento. Prima di tutto la `RecentProducts` vengono aggiornati i dati di GridView. Questo controllo GridView è stato aggiunto nell'esercitazione precedente della pagina master e visualizza i cinque prodotti aggiunti più di recente. È necessario aggiornare la griglia in modo che mostra i prezzi per questi cinque prodotti just-raddoppiato. In seguito, il `PricesDoubled` viene generato l'evento. Un riferimento alla pagina master stessa (`this`) viene inviato al gestore eventi come origine evento e un oggetto vuoto `EventArgs` oggetto viene inviato come argomenti dell'evento.

## <a name="step-4-handling-the-event-in-the-content-page"></a>Passaggio 4: La gestione dell'evento nella pagina di contenuto

A questo punto della pagina master genera relativi `PricesDoubled` evento ogni volta che il `DoublePrice` si fa clic sul controllo pulsante. Tuttavia, questo è basta: è comunque necessario gestire l'evento nel Sottoscrittore. Ciò comporta due passaggi: creazione del gestore eventi e aggiungendo il codice di cablaggio dell'evento in modo che quando viene generato l'evento viene eseguito il gestore dell'evento.

Per iniziare, creare un gestore eventi denominato `Master_PricesDoubled`. A causa di come è stato definito il `PricesDoubled` evento nella pagina master due parametri di input del gestore eventi devono essere di tipi `Object` e `EventArgs`, rispettivamente. Nella chiamata al gestore dell'evento il `ProductsGrid` GridView `DataBind` metodo riassociare i dati nella griglia.


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample10.cs)]

Il codice per il gestore eventi è stato completato ma dobbiamo ancora wire della pagina master `PricesDoubled` questo gestore dell'evento. Il sottoscrittore collega un evento a un gestore eventi tramite la sintassi seguente:


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample11.cs)]

*server di pubblicazione* è un riferimento all'oggetto che offre l'evento *eventName*, e *methodName* è il nome del gestore dell'evento definito nel Sottoscrittore che ha una firma corrispondente a il *eventDelegate*. In altre parole, se l'evento delegato viene `EventHandler`, quindi *NomeMetodo* deve essere il nome di un metodo nel Sottoscrittore che non restituisce un valore e accetta due parametri di tipi di input `Object` e `EventArgs`, rispettivamente.

Questo codice di associazione di eventi deve essere eseguito sulla prima pagina visita e durante i postback successivi e deve essere eseguita in un punto nel ciclo di vita di pagina che precede quando potrebbe essere generato l'evento. Il momento giusto per aggiungere il codice di cablaggio dell'evento è in fase di PreInit, che si verifica molto presto nel ciclo di vita della pagina.

Aprire `~/Admin/Products.aspx` e creare un `Page_PreInit` gestore dell'evento:


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample12.cs)]

Per completare questo codice di associazione è necessario un riferimento a livello di codice della pagina master dalla pagina contenuto. Come indicato nell'esercitazione precedente, esistono due modi per eseguire questa operazione:

- Eseguendo il cast di non fortemente tipizzate `Page.Master` proprietà al tipo di pagina master appropriato, o
- Aggiungendo un `@MasterType` direttiva nel `.aspx` pagina e quindi usando fortemente tipizzata `Master` proprietà.

Si userà il secondo approccio. Aggiungere il codice seguente `@MasterType` direttiva all'inizio del markup dichiarativo della pagina:


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample13.aspx)]

Quindi aggiungere il seguente codice di collegamento di eventi nel `Page_PreInit` gestore dell'evento:


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample14.cs)]

Con questo codice, il controllo GridView nella pagina di contenuto viene aggiornato ogni volta che il `DoublePrice` si fa clic sul pulsante.

Figure 8 e 9 viene illustrato questo comportamento. Figura 8 mostra la pagina alla prima visita. Si noti che i valori dei prezzi in entrambe le `RecentProducts` GridView (nella colonna sinistra della pagina master) e il `ProductsGrid` GridView (nella pagina di contenuto). Figura 9 mostra lo stesso schermata immediatamente dopo il `DoublePrice` è stato fatto clic sul pulsante. Come può notare, i nuovi prezzi vengono immediatamente riflesse in entrambi i GridView.


[![I valori di prezzo iniziale](interacting-with-the-content-page-from-the-master-page-cs/_static/image23.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image22.png)

**Figura 08**: valori di prezzo iniziali di ([fare clic per visualizzare l'immagine con dimensioni normali](interacting-with-the-content-page-from-the-master-page-cs/_static/image24.png))


[![I prezzi Just-Doubled vengono visualizzati nella finestra di GridView](interacting-with-the-content-page-from-the-master-page-cs/_static/image26.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image25.png)

**Figura 09**: i prezzi The Just-Doubled vengono visualizzati di GridView ([fare clic per visualizzare l'immagine con dimensioni normali](interacting-with-the-content-page-from-the-master-page-cs/_static/image27.png))


## <a name="summary"></a>Riepilogo

In teoria, una pagina master e le pagine contenuto sono completamente separate rispetto a altro e non richiedono alcun livello di interazione. Tuttavia, se si dispone di una pagina di contenuto che visualizza i dati che possono essere modificati dalla pagina master o pagina di contenuto o una pagina master, quindi potrebbe essere necessario disporre della pagina master avviso la pagina contenuto (e viceversa un-) quando i dati vengono modificati in modo che sia possibile aggiornare la visualizzazione. Nell'esercitazione precedente abbiamo visto come disporre di una pagina di contenuto a livello di programmazione interagire con la pagina master. in questa esercitazione è stato descritto come disporre di una pagina master avvia l'interazione.

Durante l'interazione a livello di codice tra una pagina master e di contenuto può provenire dal contenuto o pagina master, il modello di interazione utilizzato varia a seconda origine. Le differenze sono dovute al fatto che una pagina di contenuto ha una singola pagina master, ma una pagina master può avere numerose pagine di contenuto diverse. Invece di una pagina master interagisce direttamente con una pagina di contenuto, un approccio migliore consiste di generare un evento per segnalare che un'azione ha avuto luogo la pagina master. Tali pagine di contenuto che interessano l'azione è possono creare gestori eventi.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [L'accesso e aggiornamento dei dati in ASP.NET](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [Eventi e delegati](https://msdn.microsoft.com/library/17sde2xt.aspx)
- [Il passaggio di informazioni tra il contenuto e le pagine Master](http://aspnet.4guysfromrolla.com/articles/013107-1.aspx)
- [Utilizzo dei dati nelle esercitazioni ASP.NET](../../data-access/index.md)

### <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di più libri e fondatore di 4GuysFromRolla.com, ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach manualmente ASP.NET 3.5 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). È possibile contattarlo Scott [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) o il suo blog all'indirizzo [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore capo di questa esercitazione è stata Suchi Banerjee. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami un messaggio nella [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](interacting-with-the-master-page-from-the-content-page-cs.md)
> [Successivo](master-pages-and-asp-net-ajax-cs.md)
