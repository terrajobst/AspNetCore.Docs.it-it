---
uid: web-forms/overview/older-versions-getting-started/master-pages/master-pages-and-asp-net-ajax-cs
title: Pagine master e ASP.NET AJAX (c#) | Microsoft Docs
author: rick-anderson
description: Illustra le opzioni per l'uso di pagine master e ASP.NET AJAX. Vengono esaminati tramite la classe ScriptManagerProxy; viene illustrato come i vari file JS vengono caricati dependi...
ms.author: aspnetcontent
ms.date: 07/11/2008
ms.assetid: 0c55eb66-ba44-4d49-98e8-5c87fd9b1111
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/master-pages-and-asp-net-ajax-cs
msc.type: authoredcontent
ms.openlocfilehash: e8a4f9780b41c5ff77b996894d9f91a532877245
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37842484"
---
<a name="master-pages-and-aspnet-ajax-c"></a>Pagine master e ASP.NET AJAX (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare il codice](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_08_CS.zip) o [Scarica il PDF](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_08_CS.pdf)

> Illustra le opzioni per l'uso di pagine master e ASP.NET AJAX. Vengono esaminati tramite la classe ScriptManagerProxy; viene descritto il modo in cui i vari file JS vengono caricati a seconda del fatto che ScriptManager viene utilizzato nel Master pagina o pagina di contenuto.


## <a name="introduction"></a>Introduzione

Negli ultimi anni, maggiore di sviluppatori hanno creato [AJAX](http://en.wikipedia.org/wiki/Ajax_(programming))-applicazioni web abilitate. Un sito Web compatibile con AJAX utilizza una serie di tecnologie di siti web correlati a offrire un'esperienza utente più reattiva. Creazione di applicazioni basate su AJAX ASP.NET è incredibilmente facile grazie a Microsoft [framework di ASP.NET AJAX](../../../../ajax/index.md). ASP.NET AJAX è incorporato in ASP.NET 3.5 e Visual Studio 2008; è anche disponibile come download separato per le applicazioni ASP.NET 2.0.

Quando si compilano pagine web AJAX con il framework ASP.NET AJAX, è necessario aggiungere precisamente uno [controllo ScriptManager](https://msdn.microsoft.com/library/bb398863.aspx) a ogni pagina che usa il framework. Come suggerisce il nome, ScriptManager gestisce lo script lato client utilizzato nelle pagine web basate su AJAX. Come minimo, ScriptManager emette codice HTML che indica al browser di scaricare i file JavaScript che la libreria Client AJAX ASP.NET di composizione. Può anche essere utilizzato per registrare i file JavaScript personalizzati, servizi web compatibili con script e funzionalità del servizio dell'applicazione personalizzata.

Se nell'unità master utilizza sito delle pagine (come dovrebbe essere), non necessariamente occorre aggiungere un controllo ScriptManager a ogni singola pagina contenuto. piuttosto, è possibile aggiungere un controllo ScriptManager della pagina master. Questa esercitazione illustra come aggiungere il controllo ScriptManager della pagina master. Descrive anche come usare il controllo ScriptManagerProxy per registrare gli script personalizzati e i servizi script in una pagina di contenuto specifica.

> [!NOTE]
> Questa esercitazione non esplorare la progettazione o la creazione di applicazioni web basate su AJAX con il framework ASP.NET AJAX. Per altre informazioni sull'uso di AJAX, consultare il [video su ASP.NET AJAX](../../../videos/aspnet-ajax/index.md) e [esercitazioni](../aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax.md), nonché come le risorse elencate nella sezione ulteriori letture alla fine di questa esercitazione.


## <a name="examining-the-markup-emitted-by-the-scriptmanager-control"></a>Esaminare il Markup generato dal controllo ScriptManager

Il controllo ScriptManager genera markup che indica al browser di scaricare i file JavaScript che la libreria Client AJAX ASP.NET di composizione. Aggiunge anche un po' di JavaScript inline per la pagina che inizializza questa libreria. Il markup seguente mostra il contenuto che viene aggiunto all'output sottoposto a rendering di una pagina che include un controllo ScriptManager:


[!code-html[Main](master-pages-and-asp-net-ajax-cs/samples/sample1.html)]

Il `<script src="url"></script>` tag indicano al browser di scaricare ed eseguire il file JavaScript alla *url*. ScriptManager genera tre tali tag. uno fa riferimento al file `WebResource.axd`, mentre gli altri due fare riferimento al file `ScriptResource.axd`. Questi file non esistono effettivamente come file nel sito Web. Al contrario, quando arriva una richiesta per uno di questi file nel server web, il motore ASP.NET esamina la stringa di query e restituisce il contenuto JavaScript appropriato. Lo script fornito da questi tre file JavaScript esterni costituiscono la libreria Client del framework ASP.NET AJAX. L'altro `<script>` tag generati da ScriptManager includere script inline che inizializza questa libreria.

I riferimenti di script esterni e script inline generati da ScriptManager sono essenziali per una pagina che usa il framework ASP.NET AJAX, ma non è necessaria per le pagine che non usano il framework. Pertanto, si potrebbe essere motivo che è ideale per aggiungere uno ScriptManager solo alle pagine che usano il framework ASP.NET AJAX. E ciò è sufficiente, ma se si dispone di molte pagine che utilizzano il framework si otterrà l'aggiunta del controllo ScriptManager a tutte le pagine - un'attività ripetitiva, a dir poco. In alternativa, è possibile aggiungere un controllo ScriptManager della pagina master, che inserisce quindi questo script necessario in tutte le pagine di contenuto. Con questo approccio, non occorre ricordarsi di aggiungere uno ScriptManager in una nuova pagina che usa il framework ASP.NET AJAX, perché è già incluso dalla pagina master. Passaggio 1 viene illustrato come aggiungere un controllo ScriptManager della pagina master.

> [!NOTE]
> Se si prevede di includere la funzionalità AJAX all'interno dell'interfaccia utente della pagina master, quindi non sono disponibili in materia: è necessario includere ScriptManager nella pagina master.


Uno svantaggio dell'aggiunta di ScriptManager della pagina master è che lo script precedente viene emessa *ogni* pagina, indipendentemente dal fatto che secondo necessità. Ciò ovviamente comporta uno spreco della larghezza di banda per le pagine hanno ScriptManager includere (tramite la pagina master) ancora non usano le funzionalità del framework ASP.NET AJAX. Ma solo la quantità risulta sprecata della larghezza di banda?

- Il contenuto effettivo generato da ScriptManager (illustrato in precedenza) totale leggermente superiore a 1KB.
- I tre file di script esterni a cui fanno riferimento le `<script>` elemento, tuttavia, costituiscono circa 450 KB di dati non compressi; in un sito Web che utilizza la compressione gzip, è possibile ridurre la larghezza di banda totale quasi 100 KB. Tuttavia, questi file di script vengono memorizzati nella cache dal browser per un anno, vale a dire che devono solo essere scaricati una sola volta e quindi possono essere riusati in altre pagine nel sito.

Nel migliore dei casi, quindi, quando vengono memorizzati nella cache i file di script, il costo totale è 1KB, che è irrilevante. Nel peggiore dei casi, tuttavia, ovvero quando i file di script non sono ancora stati scaricati e il server web non utilizza alcuna forma di compressione - il calo della larghezza di banda è circa 450KB, che è possibile aggiungere in qualsiasi punto da un o due secondi tramite una connessione a banda larga a un minuto per  utenti tramite i modem dial-up. La buona notizia è che poiché i file di script esterni vengono memorizzati nella cache dal browser, in questo scenario peggiore si verifica raramente.

> [!NOTE]
> Se si sente ugualmente spiacevole posiziona il controllo ScriptManager nella pagina master, prendere in considerazione il Web Form (il `<form runat="server">` markup della pagina master). In ogni pagina ASP.NET che usa il modello di postback deve includere esattamente di un Web Form. Aggiunta di un modulo Web aggiunge contenuto aggiuntivo: un numero di campi modulo nascosti, di `<form>` tag, e, se necessario, una funzione JavaScript per l'avvio di un postback dallo script. Questo markup non è necessario per le pagine che non di postback. Questo markup estraneo poteva essere eliminato eliminando il Web Form dalla pagina master e aggiungerla manualmente a ogni pagina contenuto per cui è necessaria. Tuttavia, i vantaggi di avere il Web Form nella pagina master siano prevalenti rispetto agli svantaggi di averlo aggiunto inutilmente a determinate pagine di contenuto.


## <a name="step-1-adding-a-scriptmanager-control-to-the-master-page"></a>Passaggio 1: Aggiunta di un controllo ScriptManager per la pagina Master

Ogni pagina web che usa il framework ASP.NET AJAX deve contenere esattamente di un controllo ScriptManager. Questo requisito è in genere opportuno posizionare un singolo controllo ScriptManager della pagina master in modo che tutte le pagine contenuto abbiano il controllo ScriptManager incluso automaticamente. Inoltre, ScriptManager devono precedere qualsiasi controllo server AJAX ASP.NET, ad esempio i controlli UpdatePanel e UpdateProgress. Pertanto, è consigliabile inserire ScriptManager prima dei controlli ContentPlaceHolder all'interno del Web Form.

Aprire il `Site.master` pagina master e aggiungere un controllo ScriptManager per la pagina all'interno del Web Form, ma prima di `<div id="topContent">` elemento (vedere la figura 1). Se si utilizza Visual Web Developer 2008 o Visual Studio 2008, il controllo ScriptManager si trova nella casella degli strumenti nella scheda Estensioni AJAX. Se si usa Visual Studio 2005, è necessario prima di tutto installare il framework ASP.NET AJAX e aggiungere i controlli alla casella degli strumenti. Visitare il [ASP.NET AJAX Wiki](https://github.com/DevExpress/AjaxControlToolkit/wiki) per ottenere il framework di ASP.NET 2.0.

Dopo l'aggiunta di ScriptManager alla pagina, modificare relativi `ID` dal `ScriptManager1` a `MyManager`.


[![Aggiungere ScriptManager nella pagina Master](master-pages-and-asp-net-ajax-cs/_static/image2.png)](master-pages-and-asp-net-ajax-cs/_static/image1.png)

**Figura 01**: aggiungere ScriptManager nella pagina Master ([fare clic per visualizzare l'immagine con dimensioni normali](master-pages-and-asp-net-ajax-cs/_static/image3.png))


## <a name="step-2-using-the-aspnet-ajax-framework-from-a-content-page"></a>Passaggio 2: Uso del Framework ASP.NET AJAX da una pagina contenuto

Con il controllo ScriptManager aggiunto alla pagina master a questo punto possiamo aggiungere la funzionalità framework ASP.NET AJAX per qualsiasi pagina di contenuto. È possibile creare una nuova pagina ASP.NET che consente di visualizzare un prodotto selezionato in modo casuale dal database Northwind. Controllo Timer del framework ASP.NET AJAX useremo per aggiornare questa visualizzazione ogni 15 secondi, che mostra un nuovo prodotto.

Iniziare creando una nuova pagina nella directory radice denominata `ShowRandomProduct.aspx`. Non dimenticare di questa nuova pagina per associare il `Site.master` pagina master.


[![Aggiungere una nuova pagina ASP.NET nel sito Web](master-pages-and-asp-net-ajax-cs/_static/image5.png)](master-pages-and-asp-net-ajax-cs/_static/image4.png)

**Figura 02**: aggiungere una nuova pagina ASP.NET nel sito Web ([fare clic per visualizzare l'immagine con dimensioni normali](master-pages-and-asp-net-ajax-cs/_static/image6.png))


Si tenga presente che nel [ *specificazione di titolo, metatag e altre intestazioni HTML nella pagina Master* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) esercitazione è stata creata una classe di pagina base personalizzata denominata `BasePage` che ha generato il titolo della pagina se si tratta di impostare in modo non esplicito. Andare alla `ShowRandomProduct.aspx` code-behind della pagina classe e fare in modo che derivano da `BasePage` (anziché da `System.Web.UI.Page`).

Aggiornare infine il `Web.sitemap` file da includere una voce per questa lezione. Aggiungere il markup seguente sotto il `<siteMapNode>` per il servizio Master per la lezione di interazione di pagina contenuto:


[!code-xml[Main](master-pages-and-asp-net-ajax-cs/samples/sample2.xml)]

L'aggiunta di questo `<siteMapNode>` elemento si riflette nelle lezioni dell'elenco (vedere la figura 5).

### <a name="displaying-a-randomly-selected-product"></a>Visualizzazione di un prodotto selezionato in modo casuale

Tornare al `ShowRandomProduct.aspx`. Dalla finestra di progettazione, trascinare un controllo UpdatePanel dalla casella degli strumenti nel `MainContent` controllo contenuto e set relativo `ID` proprietà `ProductPanel`. UpdatePanel rappresenta un'area sullo schermo che può essere aggiornato in modo asincrono tramite un postback parziale della pagina.

La prima attività consiste nel visualizzare informazioni relative a un prodotto selezionato in modo casuale all'interno di UpdatePanel. Per iniziare, trascinare un controllo DetailsView in UpdatePanel. Impostare il controllo DetailsView `ID` proprietà `ProductInfo` e cancellare relativo `Height` e `Width` proprietà. Espandi tag di DetailsView intelligenti e, dall'elenco a discesa elenco scegliere l'origine dati, scegliere di associare un nuovo controllo SqlDataSource denominato DetailsView `RandomProductDataSource`.


[![Associare il controllo DetailsView a un nuovo controllo SqlDataSource](master-pages-and-asp-net-ajax-cs/_static/image8.png)](master-pages-and-asp-net-ajax-cs/_static/image7.png)

**Figura 03**: eseguire l'associazione DetailsView per un nuovo controllo SqlDataSource ([fare clic per visualizzare l'immagine con dimensioni normali](master-pages-and-asp-net-ajax-cs/_static/image9.png))


Configurare il controllo SqlDataSource per la connessione al database Northwind tramite il `NorthwindConnectionString` (che è stato creato nel [ *l'interazione con la pagina Master dalla pagina contenuto* ](interacting-with-the-content-page-from-the-master-page-cs.md) esercitazione). Se la configurazione dell'istruzione select sceglie di specificare un'istruzione SQL personalizzata e quindi immettere la query seguente:


[!code-sql[Main](master-pages-and-asp-net-ajax-cs/samples/sample3.sql)]

Il `TOP 1` parola chiave nel `SELECT` la clausola restituisce solo il primo record restituito dalla query. Il [ `NEWID()` funzione](https://msdn.microsoft.com/library/ms190348.aspx) genera una nuova [valore dell'identificatore univoco globale (GUID)](http://en.wikipedia.org/wiki/Globally_Unique_Identifier) e può essere usato un `ORDER BY` clausola per restituire i record della tabella in ordine casuale.


[![Configurare SqlDataSource per restituire un singolo Record selezionato in modo casuale](master-pages-and-asp-net-ajax-cs/_static/image11.png)](master-pages-and-asp-net-ajax-cs/_static/image10.png)

**Figura 04**: configurare SqlDataSource per restituire un singolo, in modo casuale i Record selezionati ([fare clic per visualizzare l'immagine con dimensioni normali](master-pages-and-asp-net-ajax-cs/_static/image12.png))


Dopo aver completato la procedura guidata, Visual Studio crea un BoundField per le due colonne restituite dalla query precedente. A questo punto markup dichiarativo della pagina dovrebbe essere simile al seguente:


[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample4.aspx)]

Figura 5 mostra il `ShowRandomProduct.aspx` pagina quando viene visualizzato tramite un browser. Fare clic sul pulsante Aggiorna del browser per ricaricare la pagina. dovrebbero vedere le `ProductName` e `UnitPrice` valori per un nuovo record selezionato in modo casuale.


[![Nome e il prezzo di un prodotto casuale viene visualizzato](master-pages-and-asp-net-ajax-cs/_static/image14.png)](master-pages-and-asp-net-ajax-cs/_static/image13.png)

**Figura 05**: nome del prodotto di oggetto casuale e prezzo viene visualizzata ([fare clic per visualizzare l'immagine con dimensioni normali](master-pages-and-asp-net-ajax-cs/_static/image15.png))


### <a name="automatically-displaying-a-new-product-every-15-seconds"></a>Automaticamente visualizzato un nuovo prodotto ogni 15 secondi

Il framework ASP.NET AJAX include un controllo Timer che esegue un postback a un'ora specificata; sul postback del Timer `Tick` viene generato l'evento. Se il controllo Timer viene posizionato all'interno di un UpdatePanel si attiva un postback parziale della pagina, durante il quale è possibile riassociare il DetailsView alla visualizzazione di un nuovo prodotto selezionato in modo casuale i dati.

A tale scopo, trascinare un Timer dalla casella degli strumenti e rilasciarlo in UpdatePanel. Modificare il Timer `ID` dal `Timer1` a `ProductTimer` e il relativo `Interval` proprietà da 60000 e 15000. Il `Interval` proprietà indica il numero di millisecondi tra i vari postback; impostandola su 15000 fa sì che il Timer attivare un postback parziale della pagina ogni 15 secondi. Markup dichiarativo del Timer a questo punto dovrebbe essere simile al seguente:


[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample5.aspx)]

Creare un gestore eventi per il Timer `Tick` evento. In questo gestore eventi è necessario riassociare i dati in DetailsView chiamando DetailsView `DataBind` (metodo). In questo modo indica a DetailsView per recuperare nuovamente i dati di controllo relativa origine dati, sulla quale verrà selezionare e visualizzare in modo casuale un nuovo record (proprio come quando ricaricare la pagina facendo clic sul pulsante Aggiorna del browser) selezionato.


[!code-csharp[Main](master-pages-and-asp-net-ajax-cs/samples/sample6.cs)]

Questo è tutto qui! Rivedere la pagina tramite un browser. Inizialmente, le informazioni di un prodotto casuale viene visualizzate. Se si osserva attenderà la schermata si noterà che, dopo 15 secondi, informazioni su un nuovo prodotto magicamente sostituiscono la visualizzazione esistente.

Per visualizzare meglio ciò che avviene qui, aggiungiamo un controllo etichetta al UpdatePanel che consente di visualizzare l'ora del che ultimo aggiornamento della visualizzazione. Aggiungere un controllo etichetta Web all'interno di UpdatePanel, impostare relativo `ID` al `LastUpdateTime`e deselezionare la relativa `Text` proprietà. Successivamente, creare un gestore eventi per il UpdatePanel `Load` evento e visualizzare l'ora corrente dell'etichetta. (Il UpdatePanel `Load` evento viene generato a ogni postback di pagina completo o parziale.)


[!code-csharp[Main](master-pages-and-asp-net-ajax-cs/samples/sample7.cs)]

Con questa modifica completa, la pagina include il tempo che è stato caricato il prodotto attualmente visualizzato. Figura 6 mostra la pagina alla prima visita. Figura 7 mostra la pagina 15 secondi in un secondo momento, dopo che il controllo Timer ha "selezionata" e UpdatePanel è stato aggiornato per visualizzare informazioni su un nuovo prodotto.


[![Un prodotto in modo casuale selezionato viene visualizzato al caricamento della pagina](master-pages-and-asp-net-ajax-cs/_static/image17.png)](master-pages-and-asp-net-ajax-cs/_static/image16.png)

**Figura 06**: prodotto in modo casuale selezionato A viene visualizzato al caricamento della pagina ([fare clic per visualizzare l'immagine con dimensioni normali](master-pages-and-asp-net-ajax-cs/_static/image18.png))


[![Ogni 15 secondi viene visualizzato un nuovo in modo casuale selezionato prodotto](master-pages-and-asp-net-ajax-cs/_static/image20.png)](master-pages-and-asp-net-ajax-cs/_static/image19.png)

**Figura 07**: ogni 15 secondi viene visualizzato un nuovo in modo casuale selezionato prodotto ([fare clic per visualizzare l'immagine con dimensioni normali](master-pages-and-asp-net-ajax-cs/_static/image21.png))


## <a name="step-3-using-the-scriptmanagerproxy-control"></a>Passaggio 3: Utilizzo del controllo ScriptManagerProxy

Insieme a includere lo script necessario per il framework ASP.NET AJAX Client Library, ScriptManager può anche registrare file JavaScript personalizzati, riferimenti a servizi Web abilitati per gli script e l'autenticazione personalizzata, autorizzazione e servizi del profilo. In genere tali personalizzazioni sono specifiche per una determinata pagina. Tuttavia, se l'oggetto personalizzato crea uno script di file, i riferimenti al servizio Web o l'autenticazione, autorizzazione o i servizi del profilo vengono fatto riferimento in ScriptManager nella pagina master quindi sono inclusi in *tutti* pagine del sito Web.

Per aggiungere le personalizzazioni correlato a ScriptManager in base a una pagina per pagina usano il controllo ScriptManagerProxy. È possibile aggiungere un controllo ScriptManagerProxy a una pagina di contenuto e quindi registrare il file JavaScript personalizzato, riferimento al servizio Web, o l'autenticazione, autorizzazione o servizio profili da ScriptManagerProxy; Questo ha l'effetto della registrazione di questi servizi per la pagina di contenuto specifico.

> [!NOTE]
> Una pagina ASP.NET può solo avere non più di un controllo ScriptManager è presentano. Pertanto, è possibile aggiungere un controllo ScriptManager a una pagina di contenuto, se il controllo ScriptManager è già definito nella pagina master. L'unico scopo di controllo ScriptManagerProxy del è in modo che gli sviluppatori possono definire ScriptManager nella pagina master, ma comunque avere la possibilità di aggiungere le personalizzazioni di ScriptManager in base a una pagina per pagina.


Per visualizzare il controllo ScriptManagerProxy in azione, è possibile potenziare UpdatePanel in `ShowRandomProduct.aspx` per includere un pulsante che usa uno script lato client per sospendere o riprendere il controllo Timer. Controllo Timer ha tre metodi di lato client che è possibile usare per ottenere questa funzionalità desiderate:

- `_startTimer()` -Avvia il controllo Timer
- `_raiseTick()` -fa sì che il controllo Timer per "tick", in tal modo postback al server e la generazione relativo `Tick` eventi sul server
- `_stopTimer()` -Arresta controllo Timer

È possibile creare un file JavaScript con una variabile denominata `timerEnabled` e una funzione denominata `ToggleTimer`. Il `timerEnabled` variabile indica se il controllo Timer è attualmente abilitato o disabilitato, il valore predefinito è true. Il `ToggleTimer` funzione accetta due parametri di input: un riferimento al lato client e il pulsante di sospensione/ripresa `id` valore del controllo Timer. Questa funzione attiva o disattiva il valore di `timerEnabled`, ottiene un riferimento al controllo Timer, avvia o arresta il Timer (a seconda del valore di `timerEnabled`) e aggiorna il testo del pulsante visualizzato "Sospendi" o "Resume". Questa funzione verrà chiamata ogni volta che viene scelto il pulsante di sospensione/ripresa.

Iniziare creando una nuova cartella nel sito Web denominato `Scripts`. Successivamente, aggiungere un nuovo file nella cartella degli script denominata `TimerScript.js` di tipo JScript File.


[![Aggiungere un nuovo File JavaScript nella cartella degli script](master-pages-and-asp-net-ajax-cs/_static/image23.png)](master-pages-and-asp-net-ajax-cs/_static/image22.png)

**Figura 08**: aggiungere un nuovo JavaScript File per il `Scripts` cartella ([fare clic per visualizzare l'immagine con dimensioni normali](master-pages-and-asp-net-ajax-cs/_static/image24.png))


[![Un nuovo JavaScript File è stato aggiunto al sito Web](master-pages-and-asp-net-ajax-cs/_static/image26.png)](master-pages-and-asp-net-ajax-cs/_static/image25.png)

**Figura 09**: un nuovo JavaScript File è stato aggiunto al sito Web ([fare clic per visualizzare l'immagine con dimensioni normali](master-pages-and-asp-net-ajax-cs/_static/image27.png))


Successivamente, aggiungere lo script seguente al file TimerScript.js:


[!code-csharp[Main](master-pages-and-asp-net-ajax-cs/samples/sample8.cs)]

È ora necessario registrare questo file JavaScript personalizzato in `ShowRandomProduct.aspx`. Tornare alla `ShowRandomProduct.aspx` e aggiungere un controllo ScriptManagerProxy alla pagina, impostare il `ID` a `MyManagerProxy`. Per registrare un JavaScript personalizzato file selezionare il controllo ScriptManagerProxy nella finestra di progettazione e quindi passare alla finestra Proprietà. Una delle proprietà è denominata script. Selezione di questa proprietà consente di visualizzare l'Editor della raccolta ScriptReference riportato nella figura 10. Fare clic sul pulsante Aggiungi per includere un nuovo riferimento a script e quindi immettere il percorso del file di script nella proprietà del percorso: `~/Scripts/TimerScript.js`.


[![Aggiungere un riferimento a Script al controllo ScriptManagerProxy](master-pages-and-asp-net-ajax-cs/_static/image29.png)](master-pages-and-asp-net-ajax-cs/_static/image28.png)

**Figura 10**: aggiungere un riferimento a Script al controllo ScriptManagerProxy ([fare clic per visualizzare l'immagine con dimensioni normali](master-pages-and-asp-net-ajax-cs/_static/image30.png))


Dopo aver aggiunto il riferimento di script del controllo ScriptManagerProxy's dichiarativo markup viene aggiornato per includere un `<Scripts>` con una singola raccolta `ScriptReference` voce, come il frammento di markup seguente illustra:


[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample9.aspx)]

Il `ScriptReference` voce istruisce il controllo ScriptManagerProxy di includere un riferimento al file JavaScript nella relativa markup sottoposto a rendering. Registrando l'oggetto personalizzato di script nel ScriptManagerProxy il `ShowRandomProduct.aspx` ora include un altro output sottoposto a rendering della pagina `<script src="url"></script>` tag: `<script src="Scripts/TimerScript.js" type="text/javascript"></script>`.

È ora possibile chiamare il `ToggleTimer` definito nella funzione `TimerScript.js` dallo script client nel `ShowRandomProduct.aspx` pagina. Aggiungere il codice HTML seguente all'interno di UpdatePanel:


[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample10.aspx)]

Ciò consente di visualizzare un pulsante con il testo "Sospensione". Ogni volta che viene scelto, la funzione JavaScript `ToggleTimer` viene chiamato, passando un riferimento al pulsante e il valore id del controllo Timer (`ProductTimer`). Si noti la sintassi per ottenere il `id` valore del controllo Timer. `<%=ProductTimer.ClientID%>` Genera il valore della `ProductTimer` del controllo Timer `ClientID` proprietà. Nel [ *denominazione degli ID di controllo nelle pagine di contenuto* ](control-id-naming-in-content-pages-cs.md) esercitazione sono illustrate le differenze tra il server-side `ID` valore e del client risultante `id` valore e in che modo `ClientID` restituisce il lato client `id`.

Figura 11 Mostra questa pagina alla prima visita tramite un browser. Il Timer è attualmente in esecuzione e aggiorna le informazioni di prodotto visualizzato ogni 15 secondi. Figura 12 mostra la schermata dopo che è stato fatto clic sul pulsante Sospendi. Facendo clic sul pulsante Sospendi, arresta il Timer e aggiorna il testo del pulsante per "Resume". Le informazioni sul prodotto aggiornare (e continuare ad aggiornare ogni 15 secondi) dopo che l'utente fa clic su Riprendi.


[![Fare clic sul pulsante Sospendi per arrestare il controllo Timer](master-pages-and-asp-net-ajax-cs/_static/image32.png)](master-pages-and-asp-net-ajax-cs/_static/image31.png)

**Figura 11**: fare clic sul pulsante Sospendi per arrestare il controllo Timer ([fare clic per visualizzare l'immagine con dimensioni normali](master-pages-and-asp-net-ajax-cs/_static/image33.png))


[![Fare clic sul pulsante di ripresa per riavviare il Timer](master-pages-and-asp-net-ajax-cs/_static/image35.png)](master-pages-and-asp-net-ajax-cs/_static/image34.png)

**Figura 12**: fare clic sul pulsante di ripresa per riavviare il Timer ([fare clic per visualizzare l'immagine con dimensioni normali](master-pages-and-asp-net-ajax-cs/_static/image36.png))


## <a name="summary"></a>Riepilogo

Durante la compilazione di applicazioni web basate su AJAX mediante il framework ASP.NET AJAX è fondamentale che ogni pagina web compatibile con AJAX include un controllo ScriptManager. Per facilitare questo processo, possiamo aggiungere un controllo ScriptManager della pagina master, anziché dover ricordare di aggiungere uno ScriptManager a ogni pagina di contenuto. Passaggio 1 è stato illustrato come aggiungere ScriptManager della pagina master durante il Step 2 preso in esame l'implementazione di funzionalità AJAX in una pagina di contenuto.

Se è necessario aggiungere gli script personalizzati, i riferimenti a servizi Web abilitati per gli script, o l'autenticazione personalizzata, autorizzazione o servizi del profilo a una particolare pagina contenuto, aggiungere un controllo ScriptManagerProxy alla pagina di contenuto e quindi configurare il personalizzazioni non esiste. Passaggio 3 esaminato come utilizzare il controllo ScriptManagerProxy di registrare un file JavaScript personalizzato in una pagina di contenuto specifica.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Framework ASP.NET AJAX](../../../../ajax/index.md)
- [Esercitazioni ASP.NET AJAX](../aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax.md)
- [Video ASP.NET AJAX](../../../videos/aspnet-ajax/index.md)
- [Interfaccia utente interattiva di compilazione con Microsoft ASP.NET AJAX](http://aspnet.4guysfromrolla.com/articles/101007-1.aspx)
- [Utilizzo di NEWID per ordinare in modo casuale i record](http://www.sqlteam.com/article/using-newid-to-randomly-sort-records)
- [Utilizzo del controllo Timer](http://aspnet.4guysfromrolla.com/articles/061808-1.aspx)

### <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di più libri e fondatore di 4GuysFromRolla.com, ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach manualmente ASP.NET 3.5 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). È possibile contattarlo Scott [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) o il suo blog all'indirizzo [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami un messaggio nella [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](interacting-with-the-content-page-from-the-master-page-cs.md)
> [Successivo](specifying-the-master-page-programmatically-cs.md)
