---
uid: web-forms/overview/older-versions-getting-started/master-pages/master-pages-and-asp-net-ajax-cs
title: Pagine master e ASP.NET AJAX (c#) | Documenti Microsoft
author: rick-anderson
description: Vengono illustrate le opzioni per l'utilizzo di ASP.NET AJAX e pagine master. Esamina l'utilizzo della classe ScriptManagerProxy; viene illustrato come i diversi file JS vengono caricati dependi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2008
ms.topic: article
ms.assetid: 0c55eb66-ba44-4d49-98e8-5c87fd9b1111
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/master-pages-and-asp-net-ajax-cs
msc.type: authoredcontent
ms.openlocfilehash: 87e5855354610723823da88ec961e7391c3f705f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30888780"
---
<a name="master-pages-and-aspnet-ajax-c"></a>Pagine master e ASP.NET AJAX (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare codice](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_08_CS.zip) o [Scarica il PDF](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_08_CS.pdf)

> Vengono illustrate le opzioni per l'utilizzo di ASP.NET AJAX e pagine master. Esamina l'utilizzo della classe ScriptManagerProxy; viene descritto come vengono caricati i vari file JS a seconda che lo ScriptManager utilizzato nello schema pagina o nella pagina contenuto.


## <a name="introduction"></a>Introduzione

Negli ultimi anni, stato costruito più sviluppatori [AJAX](http://en.wikipedia.org/wiki/Ajax_(programming))-applicazioni web. Un sito Web con supporto AJAX utilizza un numero di tecnologie web correlati a offrire un'esperienza utente più reattiva. Creazione di applicazioni con supporto AJAX ASP.NET è incredibilmente semplice grazie a Microsoft [framework ASP.NET AJAX](../../../../ajax/index.md). ASP.NET AJAX è incorporato in ASP.NET 3.5 e Visual Studio 2008. è inoltre disponibile come download separato per le applicazioni ASP.NET 2.0.

Quando si compila le pagine web AJAX con il framework ASP.NET AJAX, è necessario aggiungere esattamente un [controllo ScriptManager](https://msdn.microsoft.com/library/bb398863.aspx) per ogni pagina che utilizza il framework. Come suggerisce il nome, lo ScriptManager gestisce lo script sul lato client utilizzato in pagine web AJAX. Come minimo, ScriptManager genera codice HTML che indica al browser di scaricare i file JavaScript che la libreria Client AJAX ASP.NET di composizione. E può essere utilizzato anche per registrare i file JavaScript personalizzati, servizi web compatibili con script e funzionalità del servizio dell'applicazione personalizzata.

Se il master utilizza sito di pagine (come dovrebbe essere), non necessariamente devi aggiungere un controllo ScriptManager a ogni singola pagina contenuto. piuttosto, è possibile aggiungere un controllo ScriptManager della pagina master. In questa esercitazione viene illustrato come aggiungere il controllo ScriptManager della pagina master. Risulta inoltre descritto come utilizzare il controllo ScriptManagerProxy per registrare gli script personalizzati e i servizi di script in una pagina contenuta.

> [!NOTE]
> In questa esercitazione non esplorare la progettazione o la compilazione di applicazioni web compatibili con AJAX con il framework ASP.NET AJAX. Per ulteriori informazioni sull'utilizzo di AJAX, consultare il [video di ASP.NET AJAX](../../../videos/aspnet-ajax/index.md) e [esercitazioni](../aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax.md), nonché come le risorse elencate nella sezione di approfondimento alla fine di questa esercitazione.


## <a name="examining-the-markup-emitted-by-the-scriptmanager-control"></a>Esaminando il Markup generato dal controllo ScriptManager

Il controllo ScriptManager genera markup che indica al browser di scaricare i file JavaScript che la libreria Client AJAX ASP.NET di composizione. Aggiunge inoltre un bit di inline JavaScript alla pagina che inizializza la raccolta. Di seguito viene illustrato il contenuto aggiunto all'output del rendering di una pagina che include un controllo ScriptManager:


[!code-html[Main](master-pages-and-asp-net-ajax-cs/samples/sample1.html)]

Il `<script src="url"></script>` tag indicano al browser di scaricare ed eseguire il file JavaScript alla *url*. ScriptManager genera tre tali tag. uno fa riferimento al file `WebResource.axd`, mentre le altre due fare riferimento al file `ScriptResource.axd`. Questi file non esistono effettivamente come file nel sito Web. Al contrario, quando arriva una richiesta per uno di questi file nel server web, il motore ASP.NET esamina la stringa di query e restituisce il contenuto JavaScript appropriato. Lo script fornito da questi tre file JavaScript esterni costituiscono una libreria Client di ASP.NET AJAX framework. L'altro `<script>` tag generati da ScriptManager includono script inline che inizializza la raccolta.

I riferimenti a script esterni e script inline generato da ScriptManager sono essenziali per una pagina che utilizza il framework ASP.NET AJAX, ma non è necessaria per le pagine che non utilizzano il framework. Pertanto, si potrebbe motivo è la soluzione ideale per aggiungere solo un controllo ScriptManager a tali pagine che utilizzano il framework ASP.NET AJAX. E ciò è sufficiente, ma se si dispone di molte pagine che utilizzano il framework termine, verrà visualizzata l'aggiunta del controllo ScriptManager a tutte le pagine, un'attività ripetitiva, a dir poco. In alternativa, è possibile aggiungere un controllo ScriptManager della pagina master, che inserisce quindi questo script necessario in tutte le pagine di contenuto. Con questo approccio, non è necessario ricordare di aggiungere un controllo ScriptManager in una nuova pagina che utilizza il framework ASP.NET AJAX, perché è già incluso dalla pagina master. Passaggio 1 illustra l'aggiunta di un controllo ScriptManager della pagina master.

> [!NOTE]
> Se si prevede di includere la funzionalità AJAX all'interno dell'interfaccia utente della pagina master, è possibile scegliere in materia, è necessario includere lo ScriptManager nella pagina master.


Uno svantaggio di aggiunta di ScriptManager della pagina master è che lo script precedente viene generato *ogni* pagina, indipendentemente dal fatto che il relativo necessari. Questo porta chiaramente inutilizzato larghezza di banda per le pagine che lo ScriptManager incluso (tramite la pagina master) sono ancora stati non utilizzare qualsiasi funzionalità di ASP.NET AJAX framework. Ma solo la quantità è sprecato della larghezza di banda?

- Il contenuto effettivo generato da ScriptManager (illustrato in precedenza) totali poco 1KB.
- I tre file di script esterni a cui fa riferimento il `<script>` elemento, tuttavia, costituiscono circa 450 KB di dati non compressi, in un sito Web che utilizza la compressione gzip, è possibile ridurre la larghezza di banda totale in prossimità di 100 KB. Tuttavia, questi file di script vengono memorizzati nella cache dal browser per un anno, vale a dire che solo devono essere scaricati una sola volta e quindi possono essere riutilizzati in altre pagine nel sito.

Nel migliore dei casi, quindi, quando vengono memorizzati nella cache i file di script, il costo totale è 1KB, che è irrilevante. Nel peggiore dei casi, tuttavia, ovvero quando i file di script non sono ancora stati scaricati e il server web non utilizza alcuna forma di compressione, l'hit test della larghezza di banda è circa 450KB, che è possibile aggiungere in qualsiasi punto da un secondo o due su una connessione a banda larga fino a un minuto per  utenti tramite i modem. Buone notizie sono che perché i file di script esterni vengono memorizzati nella cache dal browser, in questo scenario peggiore si verifica raramente.

> [!NOTE]
> Se ancora preferisce inserire il controllo ScriptManager nella pagina master, prendere in considerazione il Web Form (il `<form runat="server">` markup nella pagina master). In ogni pagina ASP.NET che utilizza il modello di postback deve includere esattamente un Web Form. Aggiunta di un Form Web aggiunge contenuto aggiuntivo: un numero di campi form nascosto, il `<form>` tag, e, se necessario, una funzione JavaScript per l'avvio di un postback dallo script. In questo markup non è necessario per le pagine che non di postback. Impossibile eliminare questo markup estraneo rimuovendo il Web Form della pagina master e aggiungere manualmente in ogni pagina contenuto per cui è necessaria. Tuttavia, i vantaggi di disporre di Web Form nella pagina master siano prevalenti rispetto agli svantaggi dalla presenza di aggiungerlo inutilmente a determinate pagine di contenuto.


## <a name="step-1-adding-a-scriptmanager-control-to-the-master-page"></a>Passaggio 1: Aggiunta di un controllo ScriptManager alla pagina Master

Tutte le pagine web che utilizza il framework ASP.NET AJAX deve contenere esattamente un controllo ScriptManager. A causa di questo requisito, è in genere opportuno inserire un singolo controllo ScriptManager nella pagina master, in modo che tutte le pagine di contenuto del controllo ScriptManager incluso automaticamente. Inoltre, lo ScriptManager deve precedere qualsiasi controllo server AJAX ASP.NET, ad esempio i controlli UpdatePanel e UpdateProgress. Pertanto, è consigliabile inserire lo ScriptManager prima dei controlli ContentPlaceHolder all'interno del Web Form.

Aprire il `Site.master` pagina master e aggiungere un controllo ScriptManager nel Web Form, ma prima che il `<div id="topContent">` elemento (vedere la figura 1). Se si utilizza Visual Web Developer 2008 o Visual Studio 2008, il controllo ScriptManager si trova nella casella degli strumenti nella scheda Estensioni AJAX. Se si utilizza Visual Studio 2005, è necessario prima installare il framework ASP.NET AJAX e aggiungere i controlli alla casella degli strumenti. Visitare il [ASP.NET AJAX Wiki](https://github.com/DevExpress/AjaxControlToolkit/wiki) per ottenere il framework di ASP.NET 2.0.

Dopo aver aggiunto lo ScriptManager alla pagina, modificare il relativo `ID` da `ScriptManager1` a `MyManager`.


[![Aggiungere lo ScriptManager alla pagina Master](master-pages-and-asp-net-ajax-cs/_static/image2.png)](master-pages-and-asp-net-ajax-cs/_static/image1.png)

**Figura 01**: aggiungere lo ScriptManager alla pagina Master ([fare clic per visualizzare l'immagine ingrandita](master-pages-and-asp-net-ajax-cs/_static/image3.png))


## <a name="step-2-using-the-aspnet-ajax-framework-from-a-content-page"></a>Passaggio 2: Utilizzo di ASP.NET AJAX Framework da una pagina contenuto

Con il controllo ScriptManager aggiunto alla pagina master a questo punto possiamo aggiungere funzionalità di ASP.NET AJAX framework a una pagina contenuta. È possibile crearne una nuova pagina ASP.NET che consente di visualizzare un prodotto selezionato in modo casuale dal database Northwind. Useremo controllo Timer del framework ASP.NET AJAX per aggiornare la visualizzazione di ogni 15 secondi, con un nuovo prodotto.

Iniziare creando una nuova pagina nella directory radice denominata `ShowRandomProduct.aspx`. Non dimenticare di associare la nuova pagina per il `Site.master` pagina master.


[![Aggiungere una nuova pagina ASP.NET per il sito Web](master-pages-and-asp-net-ajax-cs/_static/image5.png)](master-pages-and-asp-net-ajax-cs/_static/image4.png)

**Figura 02**: aggiungere una nuova pagina ASP.NET per il sito Web ([fare clic per visualizzare l'immagine ingrandita](master-pages-and-asp-net-ajax-cs/_static/image6.png))


Tenere presente che nel [ *che specifica il titolo, tag Meta e altre intestazioni HTML della pagina Master* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) esercitazione è stata creata una classe della pagina base personalizzata denominata `BasePage` che ha generato il titolo della pagina se si trattasse di impostare in modo non esplicito. Passare al `ShowRandomProduct.aspx` codice della pagina classe e fare in modo che derivano da `BasePage` (anziché da `System.Web.UI.Page`).

Infine, aggiornare il `Web.sitemap` file da includere una voce per questa lezione. Aggiungere il markup seguente sotto il `<siteMapNode>` per lo schema lezione interazione pagina contenuto:


[!code-xml[Main](master-pages-and-asp-net-ajax-cs/samples/sample2.xml)]

L'aggiunta di questo `<siteMapNode>` elemento si riflette nelle lezioni dell'elenco (vedere Figura 5).

### <a name="displaying-a-randomly-selected-product"></a>Visualizzazione di un prodotto selezionato in modo casuale

Tornare al `ShowRandomProduct.aspx`. Dalla finestra di progettazione, trascinare un controllo UpdatePanel dalla casella degli strumenti nel `MainContent` controllo contenuto e impostare il relativo `ID` proprietà `ProductPanel`. UpdatePanel rappresenta una regione sullo schermo che può essere aggiornato in modo asincrono tramite un postback pagina parziale.

La prima attività consiste nel visualizzare informazioni su un prodotto selezionato in modo casuale all'interno di UpdatePanel. Per iniziare, trascinare un controllo DetailsView in UpdatePanel. Impostare il controllo DetailsView `ID` proprietà `ProductInfo` e cancellare il `Height` e `Width` proprietà. Espandere tag smart di DetailsView e, dall'elenco a discesa Scegli origine dati, scegliere di associare il controllo DetailsView a un nuovo controllo SqlDataSource denominato `RandomProductDataSource`.


[![Associare un nuovo controllo SqlDataSource controllo DetailsView.](master-pages-and-asp-net-ajax-cs/_static/image8.png)](master-pages-and-asp-net-ajax-cs/_static/image7.png)

**Figura 03**: eseguire il binding di DetailsView a un nuovo controllo SqlDataSource ([fare clic per visualizzare l'immagine ingrandita](master-pages-and-asp-net-ajax-cs/_static/image9.png))


Configurare il controllo SqlDataSource per la connessione al database Northwind tramite il `NorthwindConnectionString` (che è stato creato nel [ *l'interazione con la pagina Master dalla pagina contenuto* ](interacting-with-the-content-page-from-the-master-page-cs.md) esercitazione). Quando l'istruzione select di configurazione scegliere di specificare un'istruzione SQL personalizzata e quindi immettere la query seguente:


[!code-sql[Main](master-pages-and-asp-net-ajax-cs/samples/sample3.sql)]

Il `TOP 1` parola chiave nel `SELECT` clausola restituisce solo il primo record restituito dalla query. Il [ `NEWID()` funzione](https://msdn.microsoft.com/library/ms190348.aspx) genera una nuova [il valore di identificatore univoco globale (GUID)](http://en.wikipedia.org/wiki/Globally_Unique_Identifier) e può essere usato in un `ORDER BY` clausola per restituire i record della tabella in ordine casuale.


[![Configurare SqlDataSource per restituire un singolo Record selezionato in modo casuale](master-pages-and-asp-net-ajax-cs/_static/image11.png)](master-pages-and-asp-net-ajax-cs/_static/image10.png)

**Figura 04**: configurare SqlDataSource per restituire una singola, in modo casuale Record selezionato ([fare clic per visualizzare l'immagine ingrandita](master-pages-and-asp-net-ajax-cs/_static/image12.png))


Dopo aver completato la procedura guidata, Visual Studio crea un BoundField per le due colonne restituite dalla query precedente. Markup dichiarativo della pagina a questo punto dovrebbe essere simile al seguente:


[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample4.aspx)]

La figura 5 mostra il `ShowRandomProduct.aspx` pagina quando viene visualizzato tramite un browser. Fare clic sul pulsante Aggiorna del browser per ricaricare la pagina. verrà visualizzato il `ProductName` e `UnitPrice` i valori per un nuovo record selezionato in modo casuale.


[![Nome e il prezzo del prodotto casuale viene visualizzato](master-pages-and-asp-net-ajax-cs/_static/image14.png)](master-pages-and-asp-net-ajax-cs/_static/image13.png)

**Figura 05**: nome e il prezzo del prodotto casuale A viene visualizzato ([fare clic per visualizzare l'immagine ingrandita](master-pages-and-asp-net-ajax-cs/_static/image15.png))


### <a name="automatically-displaying-a-new-product-every-15-seconds"></a>Visualizzazione di un nuovo prodotto automaticamente ogni 15 secondi

Il framework ASP.NET AJAX include un controllo Timer che esegue un postback a un'ora specificata; nel Timer di postback `Tick` viene generato l'evento. Se il controllo Timer viene posizionato all'interno di un UpdatePanel provoca un postback pagina parziale, durante i quali è possibile riassociare i dati al controllo DetailsView per visualizzare un nuovo prodotto selezionato in modo casuale.

A tale scopo, trascinare il componente Timer dalla casella degli strumenti e rilasciarlo nell'UpdatePanel. Modificare il Timer `ID` da `Timer1` a `ProductTimer` e il relativo `Interval` proprietà 60000 e 15000. Il `Interval` proprietà indica il numero di millisecondi tra i vari postback; se è impostato su 15000 fa sì che il Timer attivare un postback pagina parziale ogni 15 secondi. Markup dichiarativo del Timer a questo punto dovrebbe essere simile al seguente:


[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample5.aspx)]

Creare un gestore eventi per il Timer `Tick` evento. In questo gestore eventi è necessario riassociare i dati di DetailsView chiamando di DetailsView `DataBind` metodo. In questo modo indica al controllo DetailsView per recuperare nuovamente i dati dal relativo controllo origine dati, che selezionare e visualizzare in modo casuale una nuova selezione di record (ad esempio quando il ricaricamento della pagina facendo clic sul pulsante Aggiorna del browser).


[!code-csharp[Main](master-pages-and-asp-net-ajax-cs/samples/sample6.cs)]

Questo è tutto qui! Rivedere la pagina tramite un browser. Inizialmente, informazioni del prodotto casuale. Se si guarda pazientemente schermata si noterà che, dopo 15 secondi, informazioni su un nuovo prodotto Authenticated sostituiscono la visualizzazione esistente.

Per visualizzare meglio ciò che accade, aggiungere un controllo etichetta a UpdatePanel che visualizza l'ora che dell'ultimo aggiornamento della visualizzazione. Aggiungere un controllo etichetta Web all'interno di UpdatePanel, impostare il relativo `ID` a `LastUpdateTime`e deselezionare la relativa `Text` proprietà. Successivamente, creare un gestore eventi per l'UpdatePanel `Load` evento e visualizzare l'ora corrente dell'etichetta. (L'UpdatePanel `Load` evento viene generato a ogni postback pagina completo o parziale.)


[!code-csharp[Main](master-pages-and-asp-net-ajax-cs/samples/sample7.cs)]

Con questa modifica è completa, la pagina include l'ora in cui che è stato caricato il prodotto attualmente visualizzato. Figura 6 mostra la pagina alla prima visita. Figura 7 mostra la pagina in un secondo momento 15 secondi dopo il controllo Timer è "selezionata" e l'UpdatePanel è stato aggiornato per visualizzare informazioni su un nuovo prodotto.


[![Un prodotto in modo casuale selezionato viene visualizzato al caricamento della pagina](master-pages-and-asp-net-ajax-cs/_static/image17.png)](master-pages-and-asp-net-ajax-cs/_static/image16.png)

**Figura 06**: in modo casuale selezionato prodotto viene visualizzato al caricamento della pagina ([fare clic per visualizzare l'immagine ingrandita](master-pages-and-asp-net-ajax-cs/_static/image18.png))


[![Ogni 15 secondi viene visualizzato un nuovo in modo casuale selezionato prodotto](master-pages-and-asp-net-ajax-cs/_static/image20.png)](master-pages-and-asp-net-ajax-cs/_static/image19.png)

**Figura 07**: ogni 15 secondi, viene visualizzato un nuovo in modo casuale selezionato prodotto ([fare clic per visualizzare l'immagine ingrandita](master-pages-and-asp-net-ajax-cs/_static/image21.png))


## <a name="step-3-using-the-scriptmanagerproxy-control"></a>Passaggio 3: Utilizzo del controllo ScriptManagerProxy

Insieme, tra cui lo script necessario per il framework ASP.NET AJAX libreria Client lo ScriptManager può registrare anche i file JavaScript personalizzati, riferimenti a servizi Web abilitati per script e l'autenticazione personalizzata, autorizzazione e servizi di profilo. In genere tali personalizzazioni sono specifiche per una determinata pagina. Tuttavia, se l'oggetto personalizzato script di file, riferimenti a servizi Web o l'autenticazione, autorizzazione o i servizi di profilo vengono fatto riferimento in ScriptManager nella pagina master quindi sono inclusi in *tutti* pagine nel sito Web.

Per aggiungere personalizzazioni ScriptManager correlate in base a una pagina per pagina utilizzano il controllo ScriptManagerProxy. È possibile aggiungere un ScriptManagerProxy a una pagina contenuto e quindi registrare il file JavaScript personalizzato, riferimento al servizio Web, o l'autenticazione, autorizzazione o servizio profilo il ScriptManagerProxy; Questo ha l'effetto della registrazione di questi servizi per la pagina di contenuto specifico.

> [!NOTE]
> Una pagina ASP.NET può solo avere non più di un controllo ScriptManager presentano. Pertanto, è possibile aggiungere un controllo ScriptManager a una pagina contenuto, se il controllo ScriptManager è già definito nella pagina master. Il solo scopo di ScriptManagerProxy è consentono agli sviluppatori di definire lo ScriptManager nella pagina master, mantenendo comunque la possibilità di aggiungere ScriptManager personalizzazioni in base a una pagina per pagina.


Per visualizzare il controllo ScriptManagerProxy in azione, si aumentano l'UpdatePanel in `ShowRandomProduct.aspx` per includere un pulsante che utilizza uno script sul lato client per sospendere o riprendere il controllo Timer. Il controllo Timer ha tre metodi sul lato client che è possibile utilizzare per ottenere questa funzionalità desiderata:

- `_startTimer()` -Avvia il controllo Timer
- `_raiseTick()` -Determina il controllo Timer "tick", in tal modo postback e generazione di relativo `Tick` eventi nel server
- `_stopTimer()` -Interrompe il controllo Timer

Creare un file JavaScript con una variabile denominata `timerEnabled` e una funzione denominata `ToggleTimer`. Il `timerEnabled` variabile indica se il controllo Timer è attualmente abilitato o disabilitato, il valore predefinito è true. Il `ToggleTimer` funzione accetta due parametri di input: un riferimento per il lato client e il pulsante di sospensione/ripresa `id` valore del controllo Timer. Questa funzione attiva o disattiva il valore di `timerEnabled`, ottiene un riferimento al controllo Timer, avvia o arresta il Timer (a seconda del valore di `timerEnabled`) e aggiorna il testo del pulsante visualizzato "Sospensione" o "Resume". Questa funzione verrà chiamata ogni volta che viene scelto il pulsante di sospensione/ripresa.

Iniziare creando una nuova cartella nel sito Web denominato `Scripts`. Successivamente, aggiungere un nuovo file nella cartella degli script denominata `TimerScript.js` di tipo JScript File.


[![Aggiungere un nuovo File JavaScript alla cartella degli script](master-pages-and-asp-net-ajax-cs/_static/image23.png)](master-pages-and-asp-net-ajax-cs/_static/image22.png)

**Figura 08**: aggiungere un nuovo File di JavaScript e il `Scripts` cartella ([fare clic per visualizzare l'immagine ingrandita](master-pages-and-asp-net-ajax-cs/_static/image24.png))


[![Un nuovo JavaScript File è stato aggiunto al sito Web](master-pages-and-asp-net-ajax-cs/_static/image26.png)](master-pages-and-asp-net-ajax-cs/_static/image25.png)

**Figura 09**: un nuovo JavaScript File è stato aggiunto al sito Web ([fare clic per visualizzare l'immagine ingrandita](master-pages-and-asp-net-ajax-cs/_static/image27.png))


Successivamente, aggiungere il seguente script al file TimerScript.js:


[!code-csharp[Main](master-pages-and-asp-net-ajax-cs/samples/sample8.cs)]

È ora necessario registrare il file JavaScript personalizzato in `ShowRandomProduct.aspx`. Tornare alla `ShowRandomProduct.aspx` e aggiungere un controllo ScriptManagerProxy alla pagina, impostare il relativo `ID` a `MyManagerProxy`. Per registrare un JavaScript personalizzato file selezionare il controllo ScriptManagerProxy nella finestra di progettazione e quindi passare alla finestra Proprietà. Una delle proprietà è denominata script. Selezione di questa proprietà consente di visualizzare l'Editor della raccolta ScriptReference illustrato nella figura 10. Fare clic sul pulsante Aggiungi per includere un riferimento a un nuovo script e quindi immettere il percorso del file di script nella proprietà percorso: `~/Scripts/TimerScript.js`.


[![Aggiungere un riferimento a Script al controllo ScriptManagerProxy](master-pages-and-asp-net-ajax-cs/_static/image29.png)](master-pages-and-asp-net-ajax-cs/_static/image28.png)

**Figura 10**: aggiungere un riferimento a Script al controllo ScriptManagerProxy ([fare clic per visualizzare l'immagine ingrandita](master-pages-and-asp-net-ajax-cs/_static/image30.png))


Dopo aver aggiunto il riferimento di script, il controllo ScriptManagerProxy's dichiarativa markup viene aggiornato per includere un `<Scripts>` insieme con una singola `ScriptReference` voce, come il seguente frammento di codice illustra:


[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample9.aspx)]

Il `ScriptReference` voce indica ScriptManagerProxy da includere un riferimento al file JavaScript nel relativo markup sottoposto a rendering. Tramite la registrazione personalizzata di script nel ScriptManagerProxy il `ShowRandomProduct.aspx` ora include un altro output di rendering della pagina `<script src="url"></script>` tag: `<script src="Scripts/TimerScript.js" type="text/javascript"></script>`.

È ora possibile chiamare il `ToggleTimer` funzione definita nel `TimerScript.js` da script client nel `ShowRandomProduct.aspx` pagina. Aggiungere il codice HTML seguente all'interno di UpdatePanel:


[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample10.aspx)]

Consente di visualizzare un pulsante con il testo "Sospensione". Ogni volta che viene selezionato, la funzione JavaScript `ToggleTimer` viene chiamato, passando un riferimento per il pulsante e il valore id del controllo Timer (`ProductTimer`). Si noti la sintassi per ottenere il `id` valore del controllo Timer. `<%=ProductTimer.ClientID%>` Genera il valore di `ProductTimer` del controllo Timer `ClientID` proprietà. Nel [ *denominazione degli ID controllo nelle pagine di contenuto* ](control-id-naming-in-content-pages-cs.md) esercitazione sono illustrate le differenze tra il lato server `ID` valore e il lato client risulta `id` valore e come `ClientID` restituisce lato client `id`.

Figura 11 Mostra questa pagina, alla prima visita tramite un browser. Il Timer è attualmente in esecuzione e aggiorna le informazioni del prodotto visualizzato ogni 15 secondi. Figura 12 illustra la schermata dopo aver scelto il pulsante Pausa. Facendo clic sul pulsante Sospendi, arresta il Timer e aggiorna il testo del pulsante per "Resume". Le informazioni sul prodotto aggiornare (e continuare ad aggiornare ogni 15 secondi) dopo che l'utente fa clic su Riprendi.


[![Fare clic sul pulsante Sospendi per arrestare il controllo Timer](master-pages-and-asp-net-ajax-cs/_static/image32.png)](master-pages-and-asp-net-ajax-cs/_static/image31.png)

**Figura 11**: fare clic sul pulsante Sospendi per arrestare il controllo Timer ([fare clic per visualizzare l'immagine ingrandita](master-pages-and-asp-net-ajax-cs/_static/image33.png))


[![Fare clic sul pulsante di ripristino per riavviare il Timer](master-pages-and-asp-net-ajax-cs/_static/image35.png)](master-pages-and-asp-net-ajax-cs/_static/image34.png)

**Figura 12**: fare clic sul pulsante di ripristino per riavviare il Timer ([fare clic per visualizzare l'immagine ingrandita](master-pages-and-asp-net-ajax-cs/_static/image36.png))


## <a name="summary"></a>Riepilogo

Durante la compilazione di applicazioni web compatibili con AJAX tramite il framework ASP.NET AJAX, è fondamentale che ogni pagina web con supporto AJAX include un controllo ScriptManager. Per semplificare questo processo, è possibile aggiungere uno ScriptManager nella pagina master, anziché dover ricordare di aggiungere un controllo ScriptManager a ogni pagina di contenuto. Passaggio 1 è stato illustrato come aggiungere ScriptManager della pagina master durante il passaggio 2 esaminato l'implementazione della funzionalità AJAX in una pagina contenuto.

Se è necessario aggiungere gli script personalizzati, i riferimenti a servizi Web abilitati per script, o l'autenticazione personalizzata, l'autorizzazione o i servizi di profilo a una particolare pagina contenuto, aggiungere un controllo ScriptManagerProxy alla pagina di contenuto e quindi configurare il personalizzazioni non esiste. Passaggio 3 esaminato come utilizzare il ScriptManagerProxy per registrare un file JavaScript personalizzato in una pagina contenuta.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [ASP.NET AJAX Framework](../../../../ajax/index.md)
- [Esercitazioni di AJAX ASP.NET](../aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax.md)
- [Video di AJAX ASP.NET](../../../videos/aspnet-ajax/index.md)
- [Interfaccia utente interattiva di compilazione con Microsoft ASP.NET AJAX](http://aspnet.4guysfromrolla.com/articles/101007-1.aspx)
- [Utilizzo di NEWID per ordinare in modo casuale i record](http://www.sqlteam.com/article/using-newid-to-randomly-sort-records)
- [Utilizzo del controllo Timer](http://aspnet.4guysfromrolla.com/articles/061808-1.aspx)

### <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di più libri e fondatore di 4GuysFromRolla, ha lavorato con tecnologie Web di Microsoft fin dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 3.5 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott può essere raggiunto al [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) o tramite il suo blog all'indirizzo [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](interacting-with-the-content-page-from-the-master-page-cs.md)
> [Successivo](specifying-the-master-page-programmatically-cs.md)
