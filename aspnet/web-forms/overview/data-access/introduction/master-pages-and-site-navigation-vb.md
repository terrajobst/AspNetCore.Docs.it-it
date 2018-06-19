---
uid: web-forms/overview/data-access/introduction/master-pages-and-site-navigation-vb
title: Pagine master e esplorazione del sito (VB) | Documenti Microsoft
author: rick-anderson
description: Una caratteristica comune dei siti Web intuitiva è che hanno uno schema di layout e la navigazione pagina coerente a livello di sito. In questa esercitazione verrà illustrato y...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 022801d8-a327-4d0c-8780-6094c9cee00d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/introduction/master-pages-and-site-navigation-vb
msc.type: authoredcontent
ms.openlocfilehash: 45fdbf70f7981c0faefef2603d21f913022e2a8f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887561"
---
<a name="master-pages-and-site-navigation-vb"></a>Pagine master e esplorazione del sito (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare App di esempio](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_3_VB.exe) o [Scarica il PDF](master-pages-and-site-navigation-vb/_static/datatutorial03vb1.pdf)

> Una caratteristica comune dei siti Web intuitiva è che hanno uno schema di layout e la navigazione pagina coerente a livello di sito. In questa esercitazione vengono esaminati come è possibile creare un aspetto coerente in tutte le pagine che possono essere facilmente aggiornate.


## <a name="introduction"></a>Introduzione

Una caratteristica comune dei siti Web intuitiva è che hanno uno schema di layout e la navigazione pagina coerente a livello di sito. ASP.NET 2.0 introduce due nuove funzionalità che semplificano notevolmente l'implementazione sia un pagina a livello di sito layout dello schema di spostamento: pagine master e l'esplorazione del sito. Pagine master consentono agli sviluppatori di creare un modello a livello di sito con le aree modificabili designate. Questo modello può essere applicato alle pagine ASP.NET nel sito. Le pagine ASP.NET è necessario solo fornire contenuto per la pagina master specificato le aree modificabili tutti gli altri markup della pagina master è identico per tutte le pagine ASP.NET che utilizzano la pagina master. Questo modello consente agli sviluppatori di definire e centralizzare un layout di pagina a livello di sito, rendendo più semplice creare un aspetto coerente in tutte le pagine che possono essere facilmente aggiornate.

Il [sistema di navigazione del sito](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx) fornisce un meccanismo per gli sviluppatori di pagina definire una mappa del sito sia un'API per tale mappa del sito, eseguire una query a livello di codice. I nuovi controlli di navigazione Web che dal Menu, TreeView e SiteMapPath semplificano il rendering di tutto o parte della mappa del sito in un elemento dell'interfaccia utente comune navigazione. Verrà usato il provider di navigazione del sito predefinito, vale a dire che la mappa del sito verrà definita in un file in formato XML.

Per rendere più fruibile sito esercitazioni illustrano questi concetti e richiederà in questa lezione, definire un layout di pagina a livello di sito, l'implementazione di una mappa del sito e l'aggiunta di interfaccia utente di spostamento. Al termine di questa esercitazione sono una progettazione elegante del sito Web per la creazione di pagine web dell'esercitazione.


[![Il risultato finale di questa esercitazione](master-pages-and-site-navigation-vb/_static/image2.png)](master-pages-and-site-navigation-vb/_static/image1.png)

**Figura 1**: il risultato di questa esercitazione End ([fare clic per visualizzare l'immagine ingrandita](master-pages-and-site-navigation-vb/_static/image3.png))


## <a name="step-1-creating-the-master-page"></a>Passaggio 1: Creazione di una pagina Master

Il primo passaggio consiste nel creare la pagina master per il sito. Al momento il sito Web è costituito solo del DataSet tipizzato (`Northwind.xsd`nel `App_Code` cartella), le classi BLL (`ProductsBLL.vb`, `CategoriesBLL.vb`e così via, tutto nel `App_Code` cartella), il database (`NORTHWND.MDF`, nel `App_Data` cartella), il file di configurazione (`Web.config`) e un file di foglio di stile CSS (`Styles.css`). Ripuliti le pagine e i file che dimostrano l'uso DAL e BLL dalle prime due esercitazioni, poiché è riesaminare tali esempi più dettagliatamente in esercitazioni future.


![I file nel progetto](master-pages-and-site-navigation-vb/_static/image4.png)

**Figura 2**: I file di parte del progetto


Per creare una pagina master, fare clic sul nome del progetto in Esplora soluzioni e scegliere Aggiungi nuovo elemento. Quindi selezionare il tipo di pagina Master dall'elenco dei modelli e denominarlo `Site.master`.


[![Aggiungere una nuova pagina Master per il sito Web](master-pages-and-site-navigation-vb/_static/image6.png)](master-pages-and-site-navigation-vb/_static/image5.png)

**Figura 3**: aggiungere una nuova pagina Master per il sito Web ([fare clic per visualizzare l'immagine ingrandita](master-pages-and-site-navigation-vb/_static/image7.png))


Definire il layout delle pagine a livello di sito nella pagina master. È possibile utilizzare l'area di progettazione e aggiungere qualsiasi controlli di Layout o Web, è necessario oppure è possibile aggiungere manualmente il markup manualmente nella visualizzazione origine. Nella pagina master usare [fogli di stile CSS](http://www.w3schools.com/css/default.asp) per il posizionamento e stili con le impostazioni di CSS definite nel file esterno `Style.css`. Sebbene non sia evidente dal tag riportati di seguito, le regole CSS sono definite in modo che la navigazione `<div>`del contenuto è posizionato in modo che viene visualizzata a sinistra e ha una larghezza fissa di 200 pixel.

Site.master


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample1.aspx)]

Una pagina master definisce il layout di pagina statici sia le aree che possono essere modificate tramite le pagine ASP.NET che utilizzano la pagina master. Queste aree modificabili del contenuto sono indicate dal controllo ContentPlaceHolder, che può essere visualizzato all'interno del contenuto `<div>`. La pagina master contiene un solo ContentPlaceHolder (`MainContent`), ma della pagina master può contenere diversi ContentPlaceHolder.

Con il markup specificato sopra, passando alla visualizzazione progettazione, viene illustrato il layout della pagina master. Tutte le pagine ASP.NET che utilizzano la pagina master avrà questo layout uniforme, con la possibilità di specificare il markup per il `MainContent` area.


[![La pagina Master, quando viene visualizzato tramite la visualizzazione di progettazione](master-pages-and-site-navigation-vb/_static/image9.png)](master-pages-and-site-navigation-vb/_static/image8.png)

**Figura 4**: la pagina Master, quando visualizzati tramite la visualizzazione della struttura ([fare clic per visualizzare l'immagine ingrandita](master-pages-and-site-navigation-vb/_static/image10.png))


## <a name="step-2-adding-a-homepage-to-the-website"></a>Passaggio 2: Aggiunta di una home page al sito Web

Con la pagina master definita, si è pronti per aggiungere le pagine ASP.NET per il sito Web. Per iniziare, aggiungere `Default.aspx`, home page del sito. Fare clic sul nome del progetto in Esplora soluzioni e scegliere Aggiungi nuovo elemento. Selezionare l'opzione di Web Form dall'elenco dei modelli del nome del file `Default.aspx`. Inoltre, controllare la casella di controllo "Seleziona pagina master".


[![Aggiungere un nuovo Web Form, il controllo della pagina master seleziona la casella di controllo](master-pages-and-site-navigation-vb/_static/image12.png)](master-pages-and-site-navigation-vb/_static/image11.png)

**Figura 5**: aggiungere un nuovo Web Form, il controllo della pagina master seleziona la casella di controllo ([fare clic per visualizzare l'immagine ingrandita](master-pages-and-site-navigation-vb/_static/image13.png))


Dopo aver fatto clic sul pulsante OK, viene chiesto di scegliere quale pagina master deve utilizzare la nuova pagina ASP.NET. Mentre nel progetto, è possibile avere più pagine master, è presente una sola.


[![Scegliere la pagina Master che devono utilizzare questa pagina ASP.NET](master-pages-and-site-navigation-vb/_static/image15.png)](master-pages-and-site-navigation-vb/_static/image14.png)

**Figura 6**: scegliere la pagina Master questo utilizzo di deve pagina ASP.NET ([fare clic per visualizzare l'immagine ingrandita](master-pages-and-site-navigation-vb/_static/image16.png))


Dopo aver selezionato la pagina master, le nuove pagine ASP.NET conterrà il markup seguente:

Default.aspx


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample2.aspx)]

Nel `@Page` direttiva vi è un riferimento al file pagina master utilizzato (`MasterPageFile="~/Site.master"`), e il markup della pagina ASP.NET contiene un controllo contenuto per ognuno dei controlli ContentPlaceHolder definiti nella pagina master, con il controllo `ContentPlaceHolderID` mapping del contenuto di controllo per uno specifico ContentPlaceHolder. Il controllo contenuto è dove si posiziona il markup si desidera visualizzare in ContentPlaceHolder corrispondente. Impostare il `@Page` della direttiva `Title` attributo alla home page e aggiungere alcuni contenuti di benvenuto al controllo del contenuto:

Default.aspx


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample3.aspx)]

Il `Title` attributo la `@Page` direttiva consente di impostare il titolo della pagina dalla pagina ASP.NET, anche se il `<title>` è definito l'elemento della pagina master. È anche possibile impostare il titolo a livello di programmazione, utilizzando `Page.Title`. Si noti inoltre che i riferimenti a fogli di stile della pagina master (ad esempio `Style.css`) vengono aggiornate automaticamente in modo che funzionino in qualsiasi pagina ASP.NET, indipendentemente dalla directory che la pagina ASP.NET è in rispetto alla pagina master.

Passando alla visualizzazione progettazione che possiamo vedere come apparirà la pagina in un browser. Si noti che nella progettazione consente di visualizzare per la pagina ASP.NET che solo le aree modificabili del contenuto sono modificabili il markup non ContentPlaceHolder definito nella pagina master sono inattivo.


[![La visualizzazione di progettazione per la pagina ASP.NET Mostra entrambe le aree modificabili e Non modificabili](master-pages-and-site-navigation-vb/_static/image18.png)](master-pages-and-site-navigation-vb/_static/image17.png)

**Figura 7**: la visualizzazione di progettazione per il ASP.NET pagina mostra sia il modificabile e Non modificabile aree ([fare clic per visualizzare l'immagine ingrandita](master-pages-and-site-navigation-vb/_static/image19.png))


Quando il `Default.aspx` viene visitata da un browser, il motore ASP.NET unisce automaticamente il contenuto della pagina pagina master e ASP. NET del contenuto e visualizza il contenuto sottoposto a merge in HTML finale che viene inviato al browser. Quando viene aggiornato il contenuto della pagina master, tutte le pagine ASP.NET che utilizzano tale pagina master avrà il relativo contenuto unito con la nuova pagina master contenuta la volta successiva in cui vengono richiesti. In breve, il modello di pagina master consente una singola pagina modello di layout per essere definita (la pagina master) le cui modifiche vengono immediatamente riflessi in tutto il sito.

## <a name="adding-additional-aspnet-pages-to-the-website"></a>Aggiunta di ulteriori pagine ASP.NET per il sito Web

È opportuno aggiungere gli stub di pagina ASP.NET aggiuntivi al sito che dovrà contenere vari report demo. Sarà presente più di just 35 demo in totale, in modo invece di creare tutte le pagine di stub consente di creare i primi. Poiché inoltre sarà diverse categorie di dimostrazioni, per migliorare la gestione dei sistemi demo aggiungere una cartella per le categorie. Aggiungere le seguenti tre cartelle per ora:

- `BasicReporting`
- `Filtering`
- `CustomFormatting`

Infine, aggiungere nuovi file, come illustrato in Esplora soluzioni nella figura 8. Quando si aggiunge a ogni file, ricordarsi di selezionare la casella di controllo "Seleziona pagina master".


![Aggiungere i file seguenti](master-pages-and-site-navigation-vb/_static/image20.png)

**Figura 8**: aggiungere i file seguenti


## <a name="step-2-creating-a-site-map"></a>Passaggio 2: Creazione di una mappa del sito

Uno dei problemi di un sito Web composto da più di un numero limitato di pagine di gestione fornisce un modo semplice per i visitatori per spostarsi tra il sito. Per iniziare, è necessario definire la struttura del sito per la navigazione. Successivamente, questa struttura deve essere convertita in elementi dell'interfaccia utente, ad esempio menu o controlli di navigazione. Infine, l'intero processo deve essere gestita e aggiornata con l'aggiunta di nuove pagine al sito e rimosse quelle esistenti. Prima di ASP.NET 2.0, gli sviluppatori dovevano creare per proprio conto la struttura del sito per la navigazione, la manutenzione e della traduzione in elementi dell'interfaccia utente. Con ASP.NET 2.0, tuttavia, gli sviluppatori possono utilizzare molto flessibile incorporato nel sistema di navigazione del sito.

Il sistema di navigazione del sito di ASP.NET 2.0 fornisce un mezzo per uno sviluppatore di definire una mappa del sito e quindi accedere a queste informazioni tramite un'API a livello di codice. ASP.NET viene fornito con un provider di mappe del sito che prevede dati mappa del sito da archiviare in un file XML formattato in modo particolare. Ma, poiché il sistema di navigazione del sito si basa sul [modello provider](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx) può essere esteso per supportare modi alternativi per la serializzazione delle informazioni della mappa del sito. Articolo di Jeff Prosise [il SQL sito mappa Provider si ve Been Waiting For](https://msdn.microsoft.com/msdnmag/issues/06/02/WickedCode/default.aspx) illustra come creare un provider di mappe del sito che archivia la mappa del sito in un database di SQL Server, un'altra opzione consiste nel creare [un provider di mappe del sito in base alle la struttura del file system](http://aspnet.4guysfromrolla.com/articles/020106-1.aspx).

Per questa esercitazione, tuttavia, utilizziamo provider mappa del sito predefinito fornito con ASP.NET 2.0. Per creare la mappa del sito, è sufficiente fare doppio clic sul nome del progetto in Esplora soluzioni, scegliere Aggiungi nuovo elemento e scegliere l'opzione mappa del sito. Lasciare il nome come `Web.sitemap` e fare clic sul pulsante Aggiungi.


[![Aggiungere una mappa del sito al progetto](master-pages-and-site-navigation-vb/_static/image22.png)](master-pages-and-site-navigation-vb/_static/image21.png)

**Figura 9**: aggiungere una mappa del sito a un progetto ([fare clic per visualizzare l'immagine ingrandita](master-pages-and-site-navigation-vb/_static/image23.png))


Il file di mappa del sito è un file XML. Si noti che Visual Studio fornisce IntelliSense per la struttura della mappa del sito. Il file di mappa del sito deve avere il `<siteMap>` nodo come nodo radice, che deve contenere esattamente un `<siteMapNode>` elemento figlio. Il primo `<siteMapNode>` elemento quindi può contenere un numero arbitrario di discendente `<siteMapNode>` elementi.

Definire la mappa del sito per simulare la struttura del file system. In altre parole, aggiungere un `<siteMapNode>` elemento per ognuna delle tre cartelle e figlio `<siteMapNode>` elementi per tutte le pagine ASP.NET in tali cartelle, come illustrato di seguito:

Web.sitemap


[!code-xml[Main](master-pages-and-site-navigation-vb/samples/sample4.xml)]

Mappa del sito definisce la struttura del sito Web, ovvero una gerarchia che descrive le varie sezioni del sito. Ogni `<siteMapNode>` elemento `Web.sitemap` rappresenta una sezione di struttura del sito.


[![Mappa del sito rappresenta una struttura per la navigazione gerarchica](master-pages-and-site-navigation-vb/_static/image25.png)](master-pages-and-site-navigation-vb/_static/image24.png)

**Figura 10**: mappa del sito rappresenta una struttura per la navigazione gerarchica ([fare clic per visualizzare l'immagine ingrandita](master-pages-and-site-navigation-vb/_static/image26.png))


ASP.NET espone la struttura della mappa del sito tramite .NET Framework [SiteMap classe](https://msdn.microsoft.com/library/system.web.sitemap.aspx). Questa classe ha un `CurrentNode` proprietà, che restituisce informazioni sulla sezione l'utente sta attualmente visitando; il `RootNode` proprietà restituisce la radice della mappa del sito (home page, in questo esempio). Sia il `CurrentNode` e `RootNode` restituiscono [SiteMapNode](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx) istanze, che dispongono di proprietà come `ParentNode`, `ChildNodes`, `NextSibling`, `PreviousSibling`e così via, che consentono di mappa del sito gerarchia.

## <a name="step-3-displaying-a-menu-based-on-the-site-map"></a>Passaggio 3: Visualizzare un Menu in base alla mappa del sito

Accesso ai dati in ASP.NET 2.0 può essere eseguita a livello di programmazione, come in ASP.NET 1. x, o in modo dichiarativo, tramite il nuovo [controlli origine dati](https://msdn.microsoft.com/library/ms227679.aspx). Esistono diversi controlli di origine di dati incorporati, ad esempio il controllo SqlDataSource, per l'accesso ai dati del database relazionale, il controllo ObjectDataSource, per l'accesso ai dati da classi e altri utenti. È inoltre possibile creare la propria [controlli origine dati personalizzati](https://msdn.microsoft.com/asp.net/reference/data/default.aspx?pull=/library/dnvs05/html/DataSourceCon1.asp).

I controlli origine dati può essere utilizzato come proxy tra la pagina ASP.NET e i dati sottostanti. Per visualizzare i dati recuperati del controllo origine dati, si sarà in genere aggiungere un altro controllo Web alla pagina e associato al controllo origine dati. Per associare un controllo Web a un controllo origine dati, è sufficiente impostare il controllo Web `DataSourceID` proprietà sul valore del controllo origine dati `ID` proprietà.

Per facilitare l'utilizzo di dati della mappa del sito, ASP.NET include il controllo SiteMapDataSource, che consente di associare un controllo Web contro la mappa del sito Web. Due controlli Web di TreeView e dal Menu vengono comunemente utilizzati per fornire un'interfaccia utente di spostamento. Per associare i dati della mappa del sito a uno di questi due controlli, aggiungere semplicemente un SiteMapDataSource alla pagina insieme a un controllo TreeView o Menu controllo cui `DataSourceID` proprietà è impostata di conseguenza. Ad esempio, è possibile aggiungere un controllo Menu della pagina master utilizzando il markup seguente:


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample5.aspx)]

Per un maggiore livello di controllo dell'HTML generato, è possibile associare il controllo SiteMapDataSource al controllo Repeater, come illustrato di seguito:


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample6.aspx)]

Il controllo SiteMapDataSource restituisce il livello di gerarchia una mappa del sito alla volta, a partire dal nodo radice della mappa del sito (home page, in questo esempio), quindi il livello successivo (Reporting di base, il filtro di report e formattazione personalizzata) e così via. Quando si associa SiteMapDataSource a un controllo Repeater, enumera il primo livello restituito e crea un'istanza di `ItemTemplate` per ogni `SiteMapNode` istanza di primo livello. Per accedere a una particolare proprietà del `SiteMapNode`, possiamo utilizzare `Eval(propertyName)`, che in modo da ottenere ogni `SiteMapNode`del `Url` e `Title` proprietà per il controllo collegamento ipertestuale.

L'esempio Ripetitore precedente verrà eseguito il rendering di markup riportato di seguito:


[!code-html[Main](master-pages-and-site-navigation-vb/samples/sample7.html)]

Questi nodi della mappa del sito (Reporting di base, il filtro di report e formattazione personalizzata) costituiscono la *secondo* livello della mappa del sito viene eseguito il rendering, non per la prima. In questo modo il controllo SiteMapDataSource `ShowStartingNode` proprietà è impostata su False, causando SiteMapDataSource ignorare il nodo radice della mappa del sito e iniziando invece la restituzione di secondo livello della gerarchia della mappa del sito.

Per visualizzare gli elementi figlio per il Reporting di base, il filtro report e la formattazione personalizzata `SiteMapNode` s, è possibile aggiungere un altro controllo Repeater al controllo Repeater iniziale `ItemTemplate`. Il secondo Repeater verrà associato il `SiteMapNode` dell'istanza `ChildNodes` proprietà, come illustrato di seguito:


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample8.aspx)]

Questi due controlli Repeater producono il markup seguente (alcuni tag è stato rimosso per brevità):


[!code-html[Main](master-pages-and-site-navigation-vb/samples/sample9.html)]

Utilizzando gli stili CSS scelto da [Rachel Andrew](http://www.rachelandrew.co.uk/)del libro [The CSS Anthology: 101 essenziali suggerimenti, consigli, &amp; delle modifiche alla](https://www.amazon.com/gp/product/0957921888/qid=1137565739/sr=8-1/ref=pd_bbs_1/103-0562306-3386214?n=507846&amp;s=books&amp;v=glance), `<ul>` e `<li>` elementi vengono applicato uno stile in modo che il markup produce il seguente output visual:


![Un Menu composto da due controlli Repeater e alcuni CSS](master-pages-and-site-navigation-vb/_static/image27.png)

**Figura 11**: un Menu composto da due controlli Repeater e alcuni CSS


Questo menu è nella pagina master e associato alla mappa del sito definita in `Web.sitemap`, ovvero che qualsiasi modifica apportata alla mappa del sito verrà riflesse immediatamente su tutte le pagine che utilizzano il `Site.master` pagina master.

## <a name="disabling-viewstate"></a>La disabilitazione di ViewState

Tutti i controlli ASP.NET possono mantenere lo stato sul [lo stato di visualizzazione](https://msdn.microsoft.com/msdnmag/issues/03/02/CuttingEdge/), che viene serializzato come un campo modulo nascosto nell'HTML. Lo stato di visualizzazione usato dai controlli per ricordare lo stato modificato a livello di codice attraverso i postback, ad esempio i dati associati a un controllo Web di dati. Mentre lo stato di visualizzazione consente di informazioni da memorizzare postback, aumenta le dimensioni dei tag che devono essere inviati al client e può causare la crescita pagina gravi se non accuratamente monitorati. I controlli Web dati particolarmente GridView sono noti per l'aggiunta di decine di kilobyte extra del markup per una pagina. Mentre un aumento di questo tipo può essere ignorabile per gli utenti a banda larga o intranet, lo stato di visualizzazione è possibile aggiungere alcuni secondi per il round trip per gli utenti remoti.

Per visualizzare l'impatto di stato di visualizzazione, visitare una pagina in un browser e visualizzare l'origine inviata dalla pagina web (in Internet Explorer, andare al menu Visualizza e scegliere l'opzione di origine). È anche possibile attivare [traccia delle pagine](https://msdn.microsoft.com/library/sfbfw58f.aspx) per visualizzare l'allocazione di stato di visualizzazione utilizzato da tutti i controlli della pagina. Le informazioni sullo stato di visualizzazione viene serializzate in un campo modulo nascosto denominato `__VIEWSTATE`, che si trova in un `<div>` elemento subito dopo l'apertura `<form>` tag. Lo stato di visualizzazione viene mantenuto solo quando è presente un Web Form in uso; Se la pagina ASP.NET non include un `<form runat="server">` nella relativa sintassi dichiarativa vi sarà un `__VIEWSTATE` campo modulo nascosto nella markup sottoposto a rendering.

Il `__VIEWSTATE` campo del form generato dalla pagina master aggiunge circa 1.800 byte per il markup della pagina generato. Questo eccesso è dovuto principalmente per il controllo Repeater, come il contenuto del controllo SiteMapDataSource viene mantenuto nello stato di visualizzazione. 1.800 byte extra potrebbe non sembrare molto ottenere entusiasti, quando si utilizza un controllo GridView con molti campi e record, lo stato di visualizzazione può causare facilmente da un fattore di 10 o più.

Lo stato di visualizzazione può essere disabilitato a livello di pagina o controllo impostando il `EnableViewState` proprietà `False`, creando così riduzione delle dimensioni del markup sottoposto a rendering. Poiché lo stato di visualizzazione per un controllo mantiene i dati associati ai dati di controllo Web postback Web i dati, quando lo stato di visualizzazione di una data di disabilitazione controllo Web i dati devono essere associati a ogni postback. In ASP.NET versione 1. x non ha questa responsabilità dello sviluppatore della pagina; ricade sullo con ASP.NET 2.0, tuttavia, i controlli Web di dati verranno riassociare al proprio controllo origine dati a ogni postback se necessario.

Per ridurre lo stato di visualizzazione della pagina verrà impostato il controllo Repeater `EnableViewState` proprietà `False`. Questa operazione può essere eseguita tramite la finestra delle proprietà nella finestra di progettazione o in modo dichiarativo nella visualizzazione origine. Dopo aver apportato questa modifica markup dichiarativo del ripetitore dovrebbe essere simile:


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample10.aspx)]

Dopo questa modifica, la pagina rendering dimensioni dello stato sono ridotto a una semplice visualizzazione 52 byte, un risparmio del 97% nello stato di visualizzazione dimensioni! In questa serie di esercitazioni lo stato di visualizzazione dei dati di controlli Web verrà disattivato per impostazione predefinita per ridurre le dimensioni del markup sottoposto a rendering. Nella maggior parte degli esempi di `EnableViewState` verrà impostata su `False` e senza alcuna indicazione. L'unico caso verrà esaminato lo stato di visualizzazione in scenari in cui deve essere abilitato affinché i dati di controllo Web per fornire le funzionalità previste.

## <a name="step-4-adding-breadcrumb-navigation"></a>Passaggio 4: Aggiunta di struttura di spostamento

Per completare la pagina master, aggiungere un elemento dell'interfaccia utente della struttura di navigazione a ogni pagina. La barra di navigazione Mostra rapidamente agli utenti la posizione corrente all'interno della gerarchia del sito. Aggiunta di una barra di navigazione in ASP.NET 2.0 è possibile solo aggiungere un controllo SiteMapPath nella pagina. non è necessario alcun codice.

Per il sito, aggiungere il controllo all'intestazione `<div>`:


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample11.aspx)]

La barra di navigazione Mostra la pagina corrente l'utente visita nella gerarchia della mappa del sito, nonché il sito "predecessori," del nodo della mappa fino alla radice (home page, in questo esempio).


![La barra di navigazione viene visualizzata la pagina corrente e gerarchia della mappa i predecessori del sito](master-pages-and-site-navigation-vb/_static/image28.png)

**Figura 12**: della barra di navigazione viene visualizzata la pagina corrente e i predecessori nel sito di gerarchia della mappa


## <a name="step-5-adding-the-default-page-for-each-section"></a>Passaggio 5: Aggiunta della pagina predefinita per ogni sezione

Le esercitazioni nel sito sono suddivisi in categorie diverse Reporting di base, filtro, la formattazione personalizzata e così via a una cartella per ogni categoria e le esercitazioni corrispondente come pagine ASP.NET all'interno di tale cartella. Ogni cartella contiene inoltre un `Default.aspx` pagina. In questa pagina predefinita, vengono visualizzate tutte le esercitazioni per la sezione corrente. Ovvero, per il `Default.aspx` nel `BasicReporting` cartella sono disponibili collegamenti a `SimpleDisplay.aspx`, `DeclarativeParams.aspx`, e `ProgrammaticParams.aspx`. In questo caso, è possibile usare nuovamente il `SiteMap` classe e un controllo Web per visualizzare queste informazioni in base alla mappa del sito di dati definiti `Web.sitemap`.

Consente di visualizzare un elenco non ordinato con un ripetitore, ma questa volta, verrà visualizzato il titolo e la descrizione delle esercitazioni. Poiché il markup e il codice per ottenere questo risultato devono essere ripetuti per ogni `Default.aspx` pagina, è possibile incapsulare questa logica dell'interfaccia utente in un [controllo utente](https://msdn.microsoft.com/library/y6wb1a0e.aspx). Creare una cartella nel sito Web denominato `UserControls` e aggiungervi un nuovo elemento di tipo controllo utente Web denominato `SectionLevelTutorialListing.ascx`e aggiungere il markup seguente:


[![Aggiungere un nuovo controllo utente Web per la cartella di controlli utente](master-pages-and-site-navigation-vb/_static/image30.png)](master-pages-and-site-navigation-vb/_static/image29.png)

**Figura 13**: aggiungere un nuovo controllo utente Web per il `UserControls` cartella ([fare clic per visualizzare l'immagine ingrandita](master-pages-and-site-navigation-vb/_static/image31.png))


SectionLevelTutorialListing.ascx


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample12.aspx)]

SectionLevelTutorialListing.ascx.vb


[!code-vb[Main](master-pages-and-site-navigation-vb/samples/sample13.vb)]

Nell'esempio precedente Ripetitore è associato il `SiteMap` dati al controllo Repeater in modo dichiarativo; `SectionLevelTutorialListing` controllo utente, tuttavia, a tale scopo a livello di codice. Nel `Page_Load` gestore dell'evento, viene eseguito un controllo per assicurarsi che questa pagina s URL esegue il mapping a un nodo nella mappa del sito. Se il controllo utente viene utilizzato in una pagina che non esiste un corrispondente `<siteMapNode>` voce, `SiteMap.CurrentNode` restituirà `Nothing` e non verrà associato alcun dato per il controllo Repeater. Supponendo che abbiamo un `CurrentNode`, viene eseguita l'associazione relativa `ChildNodes` insieme al controllo Repeater. Poiché la mappa del sito è configurata in modo che il `Default.aspx` ogni sezione della pagina è il nodo padre di tutte le esercitazioni all'interno di tale sezione, questo codice visualizza collegamenti a e descrizioni di tutte le esercitazioni, come illustrato nella cattura di schermata seguente.

Dopo aver creato il controllo Repeater, aprire il `Default.aspx` pagine in ciascuna delle cartelle, passare alla visualizzazione progettazione e trascinare il controllo utente in Esplora soluzioni nell'area di progettazione in cui si desidera che l'elenco delle esercitazioni.


[![Il controllo utente è stato aggiunto a default. aspx](master-pages-and-site-navigation-vb/_static/image33.png)](master-pages-and-site-navigation-vb/_static/image32.png)

**Figura 14**: il controllo utente è stato aggiunto alla `Default.aspx` ([fare clic per visualizzare l'immagine ingrandita](master-pages-and-site-navigation-vb/_static/image34.png))


[![Sono elencate le esercitazioni di Reporting base](master-pages-and-site-navigation-vb/_static/image36.png)](master-pages-and-site-navigation-vb/_static/image35.png)

**Figura 15**: sono elencate esercitazioni di Reporting di base ([fare clic per visualizzare l'immagine ingrandita](master-pages-and-site-navigation-vb/_static/image37.png))


## <a name="summary"></a>Riepilogo

Con la mappa del sito definito e la pagina master completa, è ora disponibile uno schema di layout e la navigazione pagina coerente per le esercitazioni relative ai dati. Indipendentemente dal fatto il numero di pagine si aggiungono al sito, l'aggiornamento delle informazioni di navigazione del sito o di layout di pagina a livello di sito è un processo semplice e rapido a causa di queste informazioni viene centralizzate. In particolare, le informazioni sul layout di pagina è definiti nella pagina master `Site.master` e il mappa del sito in `Web.sitemap`. Non è necessario scrivere *qualsiasi* codice per ottenere questo meccanismo di layout e la navigazione pagina a livello di sito e si mantengono il supporto completo WYSIWYG in Visual Studio.

Dopo aver completato il livello di accesso ai dati e il livello di logica di Business e di una pagina coerente layout ed esplorazione definito, si è pronti per iniziare a esplorare i modelli di report comuni. Nelle prossime tre esercitazioni verranno ora esaminate le attività di reporting base la visualizzazione dei dati recuperati da BLL nei controlli GridView, DetailsView e FormView.

Buona programmazione!

## <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Cenni preliminari sulle pagine Master ASP.NET](https://msdn.microsoft.com/library/wtxbf3hh.aspx)
- [Pagine master in ASP.NET 2.0](http://odetocode.com/Articles/419.aspx)
- [ASP.NET 2.0 Progettazione modelli](https://msdn.microsoft.com/asp.net/reference/design/templates/default.aspx)
- [Panoramica di spostamento sito ASP.NET](https://msdn.microsoft.com/library/e468hxky.aspx)
- [Analisi di ASP.NET 2.0 dell'esplorazione del sito](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Nuove funzionalità di navigazione di ASP.NET 2.0 del sito](https://weblogs.asp.net/scottgu/archive/2005/11/20/431019.aspx)
- [Informazioni sullo stato di visualizzazione ASP.NET](https://msdn.microsoft.com/library/default.asp?url=/library/dnaspp/html/viewstate.asp)
- [Procedura: abilitare la traccia di una pagina ASP.NET](https://msdn.microsoft.com/library/94c55d08%28VS.80%29.aspx)
- [Controlli utente ASP.NET](https://msdn.microsoft.com/library/y6wb1a0e.aspx)

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Lead revisori per questa esercitazione sono stati Liz Shulok, Dennis Patterson e Hilton Giesenow. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](creating-a-business-logic-layer-vb.md)
