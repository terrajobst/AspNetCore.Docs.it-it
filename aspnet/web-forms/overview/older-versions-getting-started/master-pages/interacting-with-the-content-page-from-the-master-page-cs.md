---
uid: web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-content-page-from-the-master-page-cs
title: L'interazione con la pagina di contenuto dalla pagina Master (c#) | Documenti Microsoft
author: rick-anderson
description: "Viene illustrato come chiamare i metodi, impostare le proprietà e così via della pagina di contenuto dal codice nella pagina Master."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2008
ms.topic: article
ms.assetid: 3282df5e-516c-4972-8666-313828b90fb5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-content-page-from-the-master-page-cs
msc.type: authoredcontent
ms.openlocfilehash: 4d7f6eeac084f3516ab470adf8973351cf08a7f1
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="interacting-with-the-content-page-from-the-master-page-c"></a>L'interazione con la pagina di contenuto dalla pagina Master (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare codice](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_07_CS.zip) o [Scarica il PDF](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_07_CS.pdf)

> Viene illustrato come chiamare i metodi, impostare le proprietà e così via della pagina di contenuto dal codice nella pagina Master.


## <a name="introduction"></a>Introduzione

L'esercitazione precedente esaminato come disporre la pagina di contenuto a livello di codice tenendo conto della pagina master. Tenere presente che è aggiornata della pagina master per includere un controllo GridView elencati i cinque più di recente aggiunti prodotti. È stato quindi creato una pagina di contenuto da cui l'utente è stato possibile aggiungere un nuovo prodotto. Al momento di aggiungere un nuovo prodotto, la pagina di contenuto necessario per indicare la pagina master per aggiornare il GridView in modo che includerebbe il prodotto appena aggiunto. Questa funzionalità è stata effettuata mediante l'aggiunta di un metodo pubblico della pagina master aggiornato con associazione a dati a GridView e quindi richiamare il metodo della pagina di contenuto.

La forma più comune di contenuto e l'interazione di pagina master provenienza dalla pagina di contenuto. Tuttavia, è possibile che la pagina master per la pagina di contenuto corrente di rouse in azione, e tale funzionalità potrebbe essere necessaria se la pagina master contiene elementi dell'interfaccia utente che consentono agli utenti di modificare i dati che viene visualizzati anche nella pagina contenuto. Si consideri una pagina contenuta che consente di visualizzare le informazioni sui prodotti in un controllo GridView di controllare e una pagina master che include un pulsante che, quando si fa clic, raddoppia i prezzi di tutti i prodotti. Molto simile all'esempio nell'esercitazione precedente, deve essere aggiornato dopo il prezzo doppio che pulsante affinché visualizzi i nuovi prezzi GridView, ma in questo scenario è la pagina master che è necessario rouse la pagina contenuta in azione.

In questa esercitazione illustra richiamare funzionalità definite nella pagina di contenuto della pagina master.

### <a name="instigating-programmatic-interaction-via-an-event-and-event-handlers"></a>Instigating a livello di programmazione tramite un evento e i gestori eventi

Richiamo di funzionalità della pagina di contenuto da una pagina master è più complessa nel caso opposto. Perché una pagina di contenuto ha una singola pagina master, quando instigating l'interazione a livello di codice della pagina di contenuto si sa quali proprietà e metodi pubblici sono a disposizione. Una pagina master, tuttavia, può avere molte pagine di contenuto diversi, ognuno con un proprio set di proprietà e metodi. Quindi, possiamo è scrivere codice nella pagina master per eseguire un'azione nella relativa pagina contenuto quando non sono disponibili informazioni sulle pagine con contenuta verranno richiamata fino all'esecuzione?

Si consideri un controllo Web ASP.NET, ad esempio il controllo pulsante. Un controllo pulsante può essere visualizzati in un numero qualsiasi di pagine ASP.NET e richiede un meccanismo mediante il quale è possibile verificare la pagina che si è stato fatto clic. Ciò avviene usando *eventi*. In particolare, il controllo pulsante genera relativo `Click` evento quando è selezionato; la pagina ASP.NET che contiene il pulsante può facoltativamente rispondere a tale notifica tramite un *gestore dell'evento*.

Questo stesso modello consente di disporre di una funzionalità di trigger pagina master nelle relative pagine di contenuto:

1. Aggiungere un evento della pagina master.
2. Genera l'evento ogni volta che la pagina master deve comunicare con la pagina contenuta. Ad esempio, se la pagina master deve avvisare relativa pagina di contenuto che l'utente è raddoppiato i prezzi, il relativo evento verrebbe generato subito dopo i prezzi sono stati raddoppiati.
3. Creare un gestore eventi in tali pagine di contenuto che sarà necessario eseguire alcune operazioni.

Il resto di questa esercitazione implementa l'esempio descritto nell'introduzione; vale a dire, una pagina contenuto in cui sono elencati i prodotti nel database e una pagina master che include un pulsante di controllo per raddoppiare i prezzi.

## <a name="step-1-displaying-products-in-a-content-page"></a>Passaggio 1: Visualizzazione dei prodotti in una pagina contenuto

Attività di primo ordine consiste nel creare una pagina contenuto in cui sono elencati i prodotti dal database Northwind. (Il database Northwind è aggiunto al progetto nell'esercitazione precedente, [ *l'interazione con la pagina Master dalla pagina contenuto*](interacting-with-the-master-page-from-the-content-page-cs.md).) Per iniziare, aggiungere una nuova pagina ASP.NET per il `~/Admin` cartella denominata `Products.aspx`, rendendo di associarlo al `Site.master` pagina master. Figura 1 Mostra Esplora soluzioni dopo l'aggiunta di questa pagina per il sito Web.


[![Aggiungere una nuova pagina ASP.NET per la cartella Admin](interacting-with-the-content-page-from-the-master-page-cs/_static/image2.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image1.png)

**Figura 01**: aggiungere una nuova pagina ASP.NET per il `Admin` cartella ([fare clic per visualizzare l'immagine ingrandita](interacting-with-the-content-page-from-the-master-page-cs/_static/image3.png))


Tenere presente che nel [ *che specifica il titolo, tag Meta e altre intestazioni HTML della pagina Master* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) esercitazione è stata creata una classe della pagina base personalizzata denominata `BasePage` che genera il titolo della pagina, in caso contrario impostare in modo esplicito. Passare al `Products.aspx` codice della pagina classe e fare in modo che derivano da `BasePage` (anziché da `System.Web.UI.Page`).

Infine, aggiornare il `Web.sitemap` file da includere una voce per questa lezione. Aggiungere il markup seguente sotto il `<siteMapNode>` per il contenuto alla lezione interazione pagina Master:


[!code-xml[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample1.xml)]

L'aggiunta di questo `<siteMapNode>` elemento si riflette nelle lezioni dell'elenco (vedere Figura 5).

Tornare al `Products.aspx`. Nel controllo del contenuto per `MainContent`, aggiungere un controllo GridView e denominarla `ProductsGrid`. Associare il controllo GridView a un nuovo controllo SqlDataSource denominato `ProductsDataSource`.


[![Associare il controllo GridView a un nuovo controllo SqlDataSource](interacting-with-the-content-page-from-the-master-page-cs/_static/image5.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image4.png)

**Figura 02**: associare il controllo GridView a un nuovo controllo SqlDataSource ([fare clic per visualizzare l'immagine ingrandita](interacting-with-the-content-page-from-the-master-page-cs/_static/image6.png))


Configurare la procedura guidata in modo che utilizzi il database Northwind. Se si è lavorato l'esercitazione precedente, si dovrebbe già disporre di una stringa di connessione denominata `NorthwindConnectionString` in `Web.config`. Scegliere la stringa di connessione dall'elenco a discesa, come mostrato nella figura 3.


[![Configurare SqlDataSource per utilizzare il Database Northwind](interacting-with-the-content-page-from-the-master-page-cs/_static/image8.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image7.png)

**Figura 03**: configurare SqlDataSource per utilizzare il Northwind Database ([fare clic per visualizzare l'immagine ingrandita](interacting-with-the-content-page-from-the-master-page-cs/_static/image9.png))


Specificare quindi il controllo origine dati `SELECT` istruzione scegliendo la tabella Products dall'elenco a discesa e restituendo la `ProductName` e `UnitPrice` colonne (vedere la figura 4). Fare clic su Avanti e quindi scegliere Fine per completare la procedura guidata Configura origine dati.


[![Restituire il ProductName e UnitPrice campi della tabella Products](interacting-with-the-content-page-from-the-master-page-cs/_static/image11.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image10.png)

**Figura 04**: restituire il `ProductName` e `UnitPrice` i campi dal `Products` tabella ([fare clic per visualizzare l'immagine ingrandita](interacting-with-the-content-page-from-the-master-page-cs/_static/image12.png))


Questo è tutto qui! Dopo aver completato la procedura guidata Visual Studio aggiunge due BoundField GridView per eseguire i due campi restituiti dal controllo SqlDataSource il mirroring. Markup dei controlli GridView e SqlDataSource segue. Figura 5 mostra i risultati quando viene visualizzato tramite un browser.


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample2.aspx)]


[![Ogni prodotto e il prezzo è elencato in GridView](interacting-with-the-content-page-from-the-master-page-cs/_static/image14.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image13.png)

**Figura 05**: ogni prodotto e il prezzo è elencato in GridView ([fare clic per visualizzare l'immagine ingrandita](interacting-with-the-content-page-from-the-master-page-cs/_static/image15.png))


> [!NOTE]
> È possibile pulire l'aspetto del controllo GridView. Alcuni suggerimenti includono la formattazione del valore di UnitPrice visualizzato come valuta e l'utilizzo di tipi di carattere e colori di sfondo per migliorare l'aspetto della griglia. Per ulteriori informazioni sulla visualizzazione e formattazione dei dati in ASP.NET, vedere il [utilizzo di una serie di esercitazioni dati](../../data-access/index.md).


## <a name="step-2-adding-a-double-prices-button-to-the-master-page"></a>Passaggio 2: Aggiunta di un pulsante di prezzi doppie alla pagina Master

L'attività successiva è per aggiungere un controllo pulsante Web al master per la pagina che, quando si fa clic, verrà raddoppiare il prezzo di tutti i prodotti nel database. Aprire il `Site.master` pagina master e trascinare un pulsante dalla casella degli strumenti nella finestra di progettazione, posizionarlo sotto il `RecentProductsDataSource` controllo SqlDataSource abbiamo aggiunto nell'esercitazione precedente. Impostare il pulsante `ID` proprietà `DoublePrice` e il relativo `Text` proprietà su "Double prezzi dei prodotti".

Successivamente, aggiungere un controllo SqlDataSource per la pagina master, denominarla `DoublePricesDataSource`. Questo SqlDataSource verrà utilizzato per eseguire il `UPDATE` doppio tutti i prezzi dell'istruzione. In particolare, è necessario impostare il relativo `ConnectionString` e `UpdateCommand` proprietà alla stringa di connessione appropriata e `UPDATE` istruzione. Quindi è necessario chiamare questo controllo SqlDataSource `Update` metodo quando il `DoublePrice` si fa clic sul pulsante. Per impostare il `ConnectionString` e `UpdateCommand` proprietà, selezionare il controllo SqlDataSource e quindi passare alla finestra Proprietà. Il `ConnectionString` elenchi di proprietà di queste stringhe di connessione già memorizzate in `Web.config` in un elenco a discesa; scegliere il `NorthwindConnectionString` opzione come illustrato nella figura 6.


[![Configurare per l'utilizzo di NorthwindConnectionString SqlDataSource](interacting-with-the-content-page-from-the-master-page-cs/_static/image17.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image16.png)

**Figura 06**: configurare SqlDataSource per utilizzare il `NorthwindConnectionString` ([fare clic per visualizzare l'immagine ingrandita](interacting-with-the-content-page-from-the-master-page-cs/_static/image18.png))


Per impostare il `UpdateCommand` proprietà, individuare l'opzione UpdateQuery nella finestra Proprietà. Questa proprietà, se selezionata, viene visualizzato un pulsante con puntini di sospensione; Fare clic su questo pulsante per visualizzare la finestra di dialogo Editor comandi e parametri illustrato nella figura 7. Digitare il comando seguente `UPDATE` istruzione nella casella di testo della finestra di dialogo:


[!code-sql[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample3.sql)]

Destinato a raddoppiare di questa istruzione, quando eseguita, il `UnitPrice` valore per ogni record di `Products` tabella.


[![Impostare la proprietà UpdateCommand del SqlDataSource](interacting-with-the-content-page-from-the-master-page-cs/_static/image20.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image19.png)

**Figura 07**: impostare SqlDataSource `UpdateCommand` proprietà ([fare clic per visualizzare l'immagine ingrandita](interacting-with-the-content-page-from-the-master-page-cs/_static/image21.png))


Dopo l'impostazione di queste proprietà, markup dichiarativo del pulsante SqlDataSource dei controlli e dovrebbe essere simile al seguente:


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample4.aspx)]

È comunque di chiamare il relativo `Update` metodo quando il `DoublePrice` si fa clic sul pulsante. Creare un `Click` gestore eventi per il `DoublePrice` pulsante e aggiungere il codice seguente:


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample5.cs)]

Per testare questa funzionalità, visitare il `~/Admin/Products.aspx` pagina abbiamo creato nel passaggio 1 e fare clic sul pulsante "Double prezzi dei prodotti". Fare clic sul pulsante provoca un postback ed esegue il `DoublePrice` del pulsante `Click` gestore dell'evento, raddoppiare i prezzi di tutti i prodotti. Rendering della pagina, quindi nuovamente il markup viene restituito e nuovamente visualizzato nel browser. GridView nella pagina di contenuto, Elenca, tuttavia, i prezzi stesso prima di "Double prezzi dei prodotti" è stato fatto clic sul pulsante. Questo avviene perché i dati inizialmente caricati in GridView fosse stato memorizzato nello stato di visualizzazione, in modo non vengono ricaricati durante i postback se non diversamente specificato. Se visita una pagina diversa, quindi tornare al `~/Admin/Products.aspx` pagina verrà visualizzato il prezzo aggiornato.

## <a name="step-3-raising-an-event-when-the-prices-are-doubled"></a>Passaggio 3: Generare un evento quando il prezzi vengono raddoppiati.

Poiché il controllo GridView nel `~/Admin/Products.aspx` pagina non rispecchiano immediatamente il raddoppiamento di prezzo, un utente può naturalmente che pensi non clic sul pulsante "Prezzi dei prodotti Double", o non funziona. Potrebbe provare facendo clic sul pulsante un maggior numero di volte, raddoppiare i prezzi più volte. Per risolvere il problema è necessario che la griglia del contenuto pagina visualizzano i nuovi prezzi immediatamente dopo che vengono raddoppiati.

Come descritto precedentemente in questa esercitazione, è necessario generare un evento nella pagina master ogni volta che l'utente fa clic il `DoublePrice` pulsante. Un evento è un modo per una classe (un server di pubblicazione di eventi) per notificare a un altro una serie di altre classi (i sottoscrittori di eventi) che si è verificato qualcosa di interessante. In questo esempio, la pagina master è il server di pubblicazione di eventi; le pagine di contenuto del rilevante quando il `DoublePrice` si fa clic sul pulsante sono i sottoscrittori.

Una classe sottoscrive un evento creando un *gestore dell'evento*, che è un metodo che viene eseguito in risposta all'evento da generare. Il server di pubblicazione definisce gli eventi che genera definendo un *delegato dell'evento*. Il delegato dell'evento specifica i parametri di input deve accettare il gestore dell'evento. In .NET Framework, i delegati non restituisce alcun valore e accetta due parametri di input:

- Un `Object`, che identifica l'origine evento, e
- Una classe derivata da`System.EventArgs`

Il secondo parametro passato al gestore eventi può includere informazioni aggiuntive sull'evento. Mentre la base `EventArgs` classe non passano tutte le informazioni, .NET Framework include una serie di classi che estendono `EventArgs` e comprendono proprietà aggiuntive. Ad esempio, un `CommandEventArgs` istanza verrà passata ai gestori di eventi che rispondono al `Command` evento e include due proprietà informativo: `CommandArgument` e `CommandName`.

> [!NOTE]
> Per ulteriori informazioni sulla creazione, la generazione e la gestione degli eventi, vedere [eventi e delegati](https://msdn.microsoft.com/library/17sde2xt.aspx) e [delegati di evento semplice inglese](http://www.codeproject.com/KB/cs/eventdelegates.aspx).


Per definire un evento utilizzare la sintassi seguente:


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample6.cs)]

Poiché è necessario solo quando l'utente ha fatto clic su un avviso nella pagina di contenuto di `DoublePrice` pulsante e non è necessario passare le eventuali altre informazioni, è possibile utilizzare il delegato dell'evento `EventHandler`, che definisce un gestore eventi che accetta come il secondo parametro di un oggetto di tipo `System.EventArgs`. Per creare l'evento nella pagina master, aggiungere la seguente riga di codice nella classe code-behind della pagina master:


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample7.cs)]

Il codice sopra riportato aggiunge un evento pubblico per la pagina master denominata `PricesDoubled`. È ora necessario generare questo evento dopo i prezzi sono stati raddoppiati. Per generare un evento utilizzare la sintassi seguente:


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample8.cs)]

Dove *mittente* e *eventArgs* sono i valori da passare al gestore dell'evento del sottoscrittore.

Aggiornamento di `DoublePrice` `Click` gestore dell'evento con il codice seguente:


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample9.cs)]

Come in precedenza, il `Click` gestore dell'evento viene avviata chiamando il `DoublePricesDataSource` del controllo SqlDataSource `Update` metodo raddoppiare i prezzi di tutti i prodotti. Sono presenti due aggiunte al gestore dell'evento in seguito. Prima di tutto, il `RecentProducts` vengono aggiornati i dati del controllo GridView. Questo controllo GridView è stato aggiunto alla pagina master nell'esercitazione precedente e visualizza i cinque prodotti aggiunto più di recente. È necessario aggiornare la griglia in modo che mostra i prezzi raddoppiato solo per questi cinque prodotti. Successivamente, il `PricesDoubled` viene generato l'evento. Un riferimento alla stessa pagina master (`this`) viene inviato al gestore eventi come origine evento e un oggetto vuoto `EventArgs` l'oggetto viene inviato come argomenti dell'evento.

## <a name="step-4-handling-the-event-in-the-content-page"></a>Passaggio 4: La gestione dell'evento nella pagina di contenuto

A questo punto della pagina master genera relativo `PricesDoubled` evento ogni volta che il `DoublePrice` si fa clic sul controllo Button. Tuttavia, questo è solo potere, è comunque necessario gestire l'evento nel Sottoscrittore. Questa operazione comporta due passaggi: creazione del gestore eventi e l'aggiunta di codice cablaggio dell'evento in modo che quando viene generato l'evento viene eseguito il gestore dell'evento.

Iniziare creando un gestore eventi denominato `Master_PricesDoubled`. A causa di come è stato definito il `PricesDoubled` evento nella pagina master due parametri di input del gestore eventi devono essere di tipi `Object` e `EventArgs`, rispettivamente. Nella chiamata al gestore dell'evento di `ProductsGrid` GridView `DataBind` metodo per riassociare i dati alla griglia.


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample10.cs)]

Il codice per il gestore dell'evento è stato completato, ma è stata ancora per collegare la pagina master `PricesDoubled` questo gestore dell'evento. Il sottoscrittore collega un evento al gestore eventi tramite la sintassi seguente:


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample11.cs)]

*server di pubblicazione* è un riferimento all'oggetto che offre l'evento *eventName*, e *NomeMetodo* è il nome del gestore eventi definito nel server di sottoscrizione che dispone di una firma corrispondente per il *eventDelegate*. In altre parole, se l'evento di delegati è `EventHandler`, quindi *NomeMetodo* deve essere il nome di un metodo nel server di sottoscrizione che non restituisce un valore e accetta due parametri di tipi di input `Object` e `EventArgs`, rispettivamente.

Questo codice di cablaggio evento deve essere eseguito nella prima visita di pagina e postback successivi e deve essere eseguita in un punto di vita che precede quando potrebbe essere generato l'evento. Consiglia di aggiungere il codice di cablaggio dell'evento è in fase di PreInit, che si verifica all'inizio nel ciclo di vita di pagina.

Aprire `~/Admin/Products.aspx` e creare un `Page_PreInit` gestore eventi:


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample12.cs)]

Per completare questo codice di collegamento è necessario un riferimento a livello di codice della pagina master dalla pagina contenuto. Come indicato nell'esercitazione precedente, sono disponibili due modi per eseguire questa operazione:

- Eseguendo il cast di fortemente tipizzato `Page.Master` proprietà al tipo di pagina master appropriato, o
- Aggiungendo un `@MasterType` direttiva di `.aspx` pagina e quindi utilizzando fortemente tipizzata `Master` proprietà.

Consente di usare l'approccio di quest'ultimo. Aggiungere il seguente `@MasterType` direttiva all'inizio del markup dichiarativo della pagina:


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample13.aspx)]

Quindi aggiungere il seguente codice di collegamento di eventi nel `Page_PreInit` gestore eventi:


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample14.cs)]

Con questo codice, il controllo GridView nella pagina di contenuto viene aggiornato ogni volta che il `DoublePrice` si fa clic sul pulsante.

Cifre 8 e 9 viene illustrato questo comportamento. Figura 8 mostra la pagina alla prima visita. Si noti che il prezzo di valori in entrambe le `RecentProducts` GridView (nella colonna sinistra della pagina master) e `ProductsGrid` GridView (nella pagina di contenuto). Figura 9 mostra la stessa schermata immediatamente dopo il `DoublePrice` ha fatto clic sul pulsante. Come si può notare, i nuovi prezzi vengono riflesse immediatamente in entrambi i GridView.


[![I valori iniziali di prezzo](interacting-with-the-content-page-from-the-master-page-cs/_static/image23.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image22.png)

**Figura 08**: i valori dei prezzi di iniziale ([fare clic per visualizzare l'immagine ingrandita](interacting-with-the-content-page-from-the-master-page-cs/_static/image24.png))


[![I prezzi Just-Doubled vengono visualizzati di GridView](interacting-with-the-content-page-from-the-master-page-cs/_static/image26.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image25.png)

**Figura 09**: The Just-Doubled prezzi vengono visualizzati di GridView ([fare clic per visualizzare l'immagine ingrandita](interacting-with-the-content-page-from-the-master-page-cs/_static/image27.png))


## <a name="summary"></a>Riepilogo

Idealmente, una pagina master e le pagine di contenuto sono completamente distinti, uno da altro e nessun livello di interazione. Tuttavia, se si dispone di una pagina master o contenuto che visualizza i dati che possono essere modificati dalla pagina master o pagina di contenuto, quindi si potrebbe essere necessario disporre della pagina master avviso pagina contenuto (o a-viceversa) quando i dati vengono modificati in modo che sia possibile aggiornare la visualizzazione. Nell'esercitazione precedente è stato illustrato come disporre di una pagina di contenuto a livello di programmazione interagire con la pagina master. in questa esercitazione è stato illustrato come avviare una pagina master l'interazione.

Durante l'interazione a livello di codice tra una pagina master e il contenuto può derivare da pagina master o di contenuto, il modello di interazione utilizzato dipende dall'origine. Le differenze sono dovute al fatto che una pagina di contenuto ha una singola pagina master, ma una pagina master può disporre di molte pagine di contenuto diversi. Anziché avere una pagina master interagire direttamente con una pagina di contenuto, un approccio migliore è di generare un evento per segnalare che un'azione ha avuto luogo la pagina master. Le pagine di contenuto che interessano l'azione è possono creare gestori eventi.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [L'accesso e l'aggiornamento dei dati in ASP.NET](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [Gli eventi e delegati](https://msdn.microsoft.com/library/17sde2xt.aspx)
- [Il passaggio di informazioni tra il contenuto e pagine Master](http://aspnet.4guysfromrolla.com/articles/013107-1.aspx)
- [Utilizzo dei dati nelle esercitazioni ASP.NET](../../data-access/index.md)

### <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di più libri e fondatore di 4GuysFromRolla, ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 3.5 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott può essere raggiunto al [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) o tramite il suo blog all'indirizzo [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore per questa esercitazione è stata Suchi Banerjee. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla[mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Precedente](interacting-with-the-master-page-from-the-content-page-cs.md)
[Successivo](master-pages-and-asp-net-ajax-cs.md)
