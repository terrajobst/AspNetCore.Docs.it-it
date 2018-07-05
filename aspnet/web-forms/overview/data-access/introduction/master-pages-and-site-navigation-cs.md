---
uid: web-forms/overview/data-access/introduction/master-pages-and-site-navigation-cs
title: Pagine master e spostamento nel sito (c#) | Microsoft Docs
author: rick-anderson
description: Una caratteristica comune dei siti Web semplici da usare è che dispongono di uno schema di layout e la navigazione pagina coerenti e a livello di sito. Questa esercitazione viene descritto come y...
ms.author: aspnetcontent
ms.date: 03/31/2010
ms.assetid: 5aee8202-a4e3-4aa9-8a95-cd5d156cea4c
msc.legacyurl: /web-forms/overview/data-access/introduction/master-pages-and-site-navigation-cs
msc.type: authoredcontent
ms.openlocfilehash: 0ad86398122f3a47721bda689b7f5afc706047bc
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816496"
---
<a name="master-pages-and-site-navigation-c"></a>Pagine master e spostamento nel sito (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'App di esempio](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_3_CS.exe) o [Scarica il PDF](master-pages-and-site-navigation-cs/_static/datatutorial03cs1.pdf)

> Una caratteristica comune dei siti Web semplici da usare è che dispongono di uno schema di layout e la navigazione pagina coerenti e a livello di sito. Questa esercitazione illustra come creare un aspetto uniforme e coerente in tutte le pagine che possono essere facilmente aggiornate.


## <a name="introduction"></a>Introduzione

Una caratteristica comune dei siti Web semplici da usare è che dispongono di uno schema di layout e la navigazione pagina coerenti e a livello di sito. ASP.NET 2.0 introduce due nuove funzionalità che semplificano notevolmente l'implementazione sia un pagina a livello di sito layout dello schema di spostamento: pagine master e spostamento del sito. Pagine master consentono agli sviluppatori di creare un modello a livello di sito con aree modificabili indicate. Questo modello può quindi essere applicato alle pagine ASP.NET nel sito. Le pagine ASP.NET devono soltanto fornire contenuto per la pagina master specificato del aree modificabili tutti gli altri tag nella pagina master è identico in tutte le pagine ASP.NET che utilizzano la pagina master. Questo modello consente agli sviluppatori di definire e centralizzare un layout di pagine nell'intero sito, rendendo più semplice creare un aspetto uniforme e coerente in tutte le pagine che possono essere facilmente aggiornate.

Il [sistema di spostamento del sito](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx) fornisce un meccanismo per sviluppatori di pagine definire una mappa del sito sia un'API per tale mappa del sito vengano sottoposte a query a livello di codice. I nuovi controlli Web di navigazione, che menu, TreeView e SiteMapPath semplificano il rendering di tutta o parte della mappa del sito in un comune elemento di interfaccia utente di navigazione. Verrà usato il provider di navigazione del sito predefinito, vale a dire che la mappa del sito verrà definita in un file di formato XML.

Per illustrare questi concetti e rendere più facilmente utilizzabili sito Web di esercitazioni, è possibile impiegare in questa lezione definizione di un layout di pagine nell'intero sito, l'implementazione di una mappa del sito e aggiunge la navigazione dell'interfaccia utente. Al termine di questa esercitazione avremo una progettazione elegante del sito Web per la compilazione alle pagine web tutorial.


[![Il risultato finale di questa esercitazione](master-pages-and-site-navigation-cs/_static/image2.png)](master-pages-and-site-navigation-cs/_static/image1.png)

**Figura 1**: il risultato di questa esercitazione End ([fare clic per visualizzare l'immagine con dimensioni normali](master-pages-and-site-navigation-cs/_static/image3.png))


## <a name="step-1-creating-the-master-page"></a>Passaggio 1: Creazione della pagina Master

Il primo passaggio consiste nel creare la pagina master per il sito. Attualmente il nostro sito Web è costituito solo del DataSet tipizzato (`Northwind.xsd`, nella `App_Code` cartella), le classi BLL (`ProductsBLL.cs`, `CategoriesBLL.cs`e così via, tutto nel `App_Code` cartella), il database (`NORTHWND.MDF`, nel `App_Data` cartella), il file di configurazione (`Web.config`) e un file di foglio di stile CSS (`Styles.css`). Sono state quindi eliminate le pagine e i file che dimostrano l'uso di DAL e BLL dalle prime due esercitazioni poiché abbiamo verranno riesaminati più tali esempi più dettagliatamente in esercitazioni future.


![I file nel progetto](master-pages-and-site-navigation-cs/_static/image4.png)

**Figura 2**: I file nel progetto


Per creare una pagina master, fare doppio clic sul nome del progetto in Esplora soluzioni e scegliere Aggiungi nuovo elemento. Quindi selezionare il tipo di pagina Master dall'elenco dei modelli e denominarlo `Site.master`.


[![Aggiungere una nuova pagina Master al sito Web](master-pages-and-site-navigation-cs/_static/image6.png)](master-pages-and-site-navigation-cs/_static/image5.png)

**Figura 3**: aggiungere una nuova pagina Master al sito Web ([fare clic per visualizzare l'immagine con dimensioni normali](master-pages-and-site-navigation-cs/_static/image7.png))


Definire il layout delle pagine nell'intero sito nella pagina master. È possibile usare la visualizzazione di progettazione e aggiungere i controlli Web o Layout è necessario oppure è possibile aggiungere manualmente il markup manualmente nella vista origine. Nella pagina master utilizzerò [fogli di stile CSS](http://www.w3schools.com/css/default.asp) per il posizionamento e gli stili con le impostazioni CSS definite nel file esterno `Style.css`. Sebbene non sia evidente dai tag riportati di seguito, le regole CSS sono definite in modo che la navigazione `<div>`del contenuto sia sempre posizionato in modo che viene visualizzata a sinistra e abbia una larghezza fissa di 200 pixel.

Site.master


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample1.aspx)]

Una pagina master definisce sia il layout delle pagine statico sia le aree che possono essere modificate tramite le pagine ASP.NET che utilizzano la pagina master. Queste aree con contenuto modificabile sono indicate dal controllo ContentPlaceHolder, che può essere visualizzato all'interno del contenuto `<div>`. Questa pagina master contiene un solo ContentPlaceHolder (`MainContent`), ma della pagina master può contenere diversi ContentPlaceHolder.

Con i tag inseriti precedentemente, passando alla visualizzazione progettazione, viene illustrato il layout della pagina master. Tutte le pagine ASP.NET che utilizzano tale pagina master avrà questo layout uniforme, con la possibilità di specificare i tag per il `MainContent` area.


[![Nella pagina Master visualizzata mediante la visualizzazione di progettazione](master-pages-and-site-navigation-cs/_static/image9.png)](master-pages-and-site-navigation-cs/_static/image8.png)

**Figura 4**: la pagina Master, quando visualizzati tramite la visualizzazione di progettazione ([fare clic per visualizzare l'immagine con dimensioni normali](master-pages-and-site-navigation-cs/_static/image10.png))


## <a name="step-2-adding-a-homepage-to-the-website"></a>Passaggio 2: Aggiunta di una home page al sito Web

Con la pagina master definita, siamo pronti per aggiungere le pagine ASP.NET per il sito Web. Iniziamo aggiungendo `Default.aspx`, home page del nostro sito Web. Fare doppio clic sul nome del progetto in Esplora soluzioni e scegliere Aggiungi nuovo elemento. Scegli l'opzione Web Form dall'elenco dei modelli e nome file `Default.aspx`. Inoltre, controllare la casella di controllo "Seleziona pagina master".


[![Aggiungere un nuovo Web Form, selezionando la casella di controllo della Seleziona pagina master](master-pages-and-site-navigation-cs/_static/image12.png)](master-pages-and-site-navigation-cs/_static/image11.png)

**Figura 5**: aggiungere un nuovo Web Form, selezionando la casella di controllo della Seleziona pagina master ([fare clic per visualizzare l'immagine con dimensioni normali](master-pages-and-site-navigation-cs/_static/image13.png))


Dopo aver fatto clic sul pulsante OK, viene richiesto di scegliere quale pagina master deve usare questa nuova pagina ASP.NET. Mentre nel progetto, è possibile avere più pagine master, abbiamo solo uno.


[![Scegliere la pagina Master che deve usare questa pagina ASP.NET](master-pages-and-site-navigation-cs/_static/image15.png)](master-pages-and-site-navigation-cs/_static/image14.png)

**Figura 6**: scegliere questa ASP.NET pagina deve usare la pagina Master ([fare clic per visualizzare l'immagine con dimensioni normali](master-pages-and-site-navigation-cs/_static/image16.png))


Dopo aver selezionato la pagina master, le nuove pagine ASP.NET conterrà il markup seguente:

Default.aspx


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample2.aspx)]

Nel `@Page` direttiva vi è un riferimento al file pagina master usato (`MasterPageFile="~/Site.master"`), e il markup della pagina ASP.NET contiene un controllo contenuto per ognuno dei controlli ContentPlaceHolder definiti nella pagina master, con il controllo `ContentPlaceHolderID` mapping del controllo Content su uno specifico ContentPlaceHolder. Il controllo contenuto corrisponde alla posizione il markup da visualizzare nel corrispondente ContentPlaceHolder. Impostare il `@Page` della direttiva `Title` attributo alla home page e aggiungere alcuni contenuti benvenuto al controllo contenuto:

Default.aspx


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample3.aspx)]

Il `Title` attributo la `@Page` direttiva consente di impostare il titolo della pagina dalla pagina ASP.NET, anche se il `<title>` elemento è definito nella pagina master. È inoltre possibile impostare il titolo a livello di programmazione, utilizzando `Page.Title`. Si noti anche che i riferimenti della pagina master ai fogli di stile (ad esempio `Style.css`) vengono aggiornate automaticamente in modo che funzionano in qualsiasi pagina ASP.NET, indipendentemente dalla directory che è la pagina ASP.NET nel relativo alla pagina master.

Passando alla visualizzazione progettazione che è possibile vedere come apparirà la pagina in un browser. Si noti che la progettazione consente di visualizzare per la pagina ASP.NET che solo le aree con contenuto modificabile sono modificabili di non dal tipo ContentPlaceHolder nella pagina master sono disabilitato.


[![La visualizzazione di progettazione per la pagina ASP.NET Mostra entrambe le aree modificabili e Non modificabili](master-pages-and-site-navigation-cs/_static/image18.png)](master-pages-and-site-navigation-cs/_static/image17.png)

**Figura 7**: Non-Editable aree e la visualizzazione di progettazione per il ASP.NET pagina mostra sia la Editable ([fare clic per visualizzare l'immagine con dimensioni normali](master-pages-and-site-navigation-cs/_static/image19.png))


Quando il `Default.aspx` viene visitata da un browser, il motore ASP.NET unisce automaticamente il contenuto della pagina pagina master e ASP. NET del contenuto e visualizza il contenuto unito HTML finale inviarlo al browser richiedente. Quando viene aggiornato il contenuto della pagina master, tutte le pagine ASP.NET che utilizzano tale pagina master avrà unito con la nuova pagina master contenuta la volta successiva in cui vengono richiesti. In breve, il modello di pagina master consente una singola pagina modello di layout definito (la pagina master) le cui modifiche vengono applicate immediatamente nell'intero sito.

## <a name="adding-additional-aspnet-pages-to-the-website"></a>Aggiunta di ulteriori pagine ASP.NET per il sito Web

Di seguito viene descritto come aggiungere ulteriori stub di pagina ASP.NET nel sito che conterranno infine le varie dimostrazioni dei rapporti. Saranno disponibili più semplicemente 35 dimostrazioni in totale, pertanto, anziché creare tutte le pagine è possibile creare le prime. Poiché saranno anche disponibili diverse categorie di dimostrazioni, per gestire meglio le demo aggiungere una cartella per le categorie. Aggiungere le seguenti tre cartelle per ora:

- `BasicReporting`
- `Filtering`
- `CustomFormatting`

Infine, aggiungere nuovi file, come mostrato in Esplora soluzioni nella figura 8. Quando si aggiunge ogni file, ricordarsi di selezionare la casella di controllo "Seleziona pagina master".


![Aggiungere i file seguenti](master-pages-and-site-navigation-cs/_static/image20.png)

**Figura 8**: aggiungere i file seguenti


## <a name="step-2-creating-a-site-map"></a>Passaggio 2: Creazione di una mappa del sito

Una delle sfide di gestione di un sito Web composto da più di un numero limitato di pagine consiste nel fornire un modo semplice per i visitatori per spostarsi tra il sito. Per iniziare, è necessario definire la struttura di spostamento del sito. Successivamente, questa struttura deve essere convertita in elementi dell'interfaccia utente per lo spostamento, come i menu o percorsi di navigazione. Infine, l'intero processo deve essere gestita e aggiornata con l'aggiunta di nuove pagine al sito e rimosse quelle esistenti. Prima di ASP.NET 2.0, gli sviluppatori dovevano creare per proprio conto la struttura per la navigazione del sito, occupandosi della gestione e della traduzione in elementi dell'interfaccia utente per lo spostamento. Con ASP.NET 2.0, tuttavia, gli sviluppatori possono usare molto flessibile compilata nel sistema di spostamento.

Il sistema di spostamento sito ASP.NET 2.0 fornisce un mezzo per uno sviluppatore di definire una mappa del sito e quindi accedere a queste informazioni tramite un'API a livello di codice. ASP.NET viene fornito con un provider della mappa del sito che si aspetta che i dati della mappa del sito da archiviare in un file XML formattato in modo particolare. Tuttavia, poiché il sistema di spostamento del sito si basa il [modello di provider](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx) può essere esteso per supportare modi alternativi per la serializzazione delle informazioni della mappa del sito. Articolo di Prosise [The SQL Site Map Provider è ve Been Waiting For](https://msdn.microsoft.com/msdnmag/issues/06/02/WickedCode/default.aspx) illustra come creare un provider di mappa del sito che archivia la mappa del sito in un database di SQL Server, un'altra opzione consiste nel creare [base a un provider di mappa del sito la struttura del file system](http://aspnet.4guysfromrolla.com/articles/020106-1.aspx).

Per questa esercitazione, tuttavia, è possibile usare il provider della mappa del sito predefinito fornito con ASP.NET 2.0. Per creare la mappa del sito, è sufficiente fare doppio clic sul nome del progetto in Esplora soluzioni, scegliere Aggiungi nuovo elemento e scegliere l'opzione mappa del sito. Lasciare il nome come `Web.sitemap` e fare clic sul pulsante Aggiungi.


[![Aggiungere una mappa del sito al progetto](master-pages-and-site-navigation-cs/_static/image22.png)](master-pages-and-site-navigation-cs/_static/image21.png)

**Figura 9**: aggiungere una mappa del sito del progetto ([fare clic per visualizzare l'immagine con dimensioni normali](master-pages-and-site-navigation-cs/_static/image23.png))


Il file di mappa del sito è un file XML. Si noti che Visual Studio offre IntelliSense per la struttura della mappa del sito. Il file di mappa del sito deve avere il `<siteMap>` nodo sotto il nodo radice, che deve contenere esattamente un `<siteMapNode>` elemento figlio. Primo `<siteMapNode>` elemento quindi può contenere un numero arbitrario di discendente `<siteMapNode>` elementi.

Definire la mappa del sito per riprodurre la struttura del file system. Vale a dire, aggiungere un `<siteMapNode>` per ognuno dei tre cartelle ed figlio `<siteMapNode>` elementi per tutte le pagine ASP.NET in tali cartelle, come segue:

Web.sitemap


[!code-xml[Main](master-pages-and-site-navigation-cs/samples/sample4.xml)]

Mappa del sito definisce la struttura del sito Web, ovvero una gerarchia che descrive le diverse sezioni del sito. Ciascuna `<siteMapNode>` elemento `Web.sitemap` rappresenta una sezione nella struttura di spostamento del sito.


[![Mappa del sito rappresenta una struttura per la navigazione gerarchica](master-pages-and-site-navigation-cs/_static/image25.png)](master-pages-and-site-navigation-cs/_static/image24.png)

**Figura 10**: la mappa del sito rappresenta una struttura di spostamento gerarchica ([fare clic per visualizzare l'immagine con dimensioni normali](master-pages-and-site-navigation-cs/_static/image26.png))


ASP.NET espone la struttura della mappa del sito tramite .NET Framework [SiteMap classe](https://msdn.microsoft.com/library/system.web.sitemap.aspx). Questa classe ha un `CurrentNode` proprietà, che restituisce informazioni sulla sezione l'utente sta attualmente visitando; il `RootNode` proprietà restituisce la radice della mappa del sito (Home, in questo esempio). Sia la `CurrentNode` e `RootNode` restituiscono [SiteMapNode](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx) istanze, che dispongono di proprietà, ad esempio `ParentNode`, `ChildNodes`, `NextSibling`, `PreviousSibling`e così via, che consentono la mappa del sito gerarchia da esaminare.

## <a name="step-3-displaying-a-menu-based-on-the-site-map"></a>Passaggio 3: Visualizzare un Menu basato sulla mappa del sito

Accesso ai dati in ASP.NET 2.0 può essere eseguita a livello di programmazione, come in ASP.NET 1.x, oppure in modo dichiarativo, tramite la nuova [controlli origine dati](https://msdn.microsoft.com/library/ms227679.aspx). Esistono diversi controlli origine dati incorporati, ad esempio il controllo SqlDataSource, per l'accesso ai dati del database relazionale, il controllo ObjectDataSource, per l'accesso ai dati da classi e altri utenti. È anche possibile creare una propria [controlli origine dati personalizzati](https://msdn.microsoft.com/asp.net/reference/data/default.aspx?pull=/library/dnvs05/html/DataSourceCon1.asp).

I controlli origine dati agiscono da proxy tra la pagina ASP.NET e i dati sottostanti. Per visualizzare i dati recuperati a un controllo origine dati, si sarà in genere aggiungere un altro controllo Web alla pagina e associarlo al controllo origine dati. Per associare un controllo Web a un controllo origine dati, è sufficiente impostare il controllo Web `DataSourceID` sul valore del controllo origine dati `ID` proprietà.

Per agevolare l'uso con i dati della mappa del sito, ASP.NET include il controllo SiteMapDataSource, che consente di associare un controllo Web per la mappa del sito Web. Due controlli Web TreeView e Menu vengono comunemente utilizzati per fornire un'interfaccia utente di navigazione. Per associare i dati della mappa del sito a uno di questi due controlli, è sufficiente aggiungere un controllo SiteMapDataSource alla pagina insieme a un controllo TreeView o Menu controllo la cui `DataSourceID` proprietà viene impostata di conseguenza. È ad esempio, potremmo aggiungere un controllo Menu alla pagina master utilizzando il markup seguente:


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample5.aspx)]

Per un miglior grado di controllo dell'HTML emesso, è possibile associare il controllo SiteMapDataSource al controllo Repeater, come illustrato di seguito:


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample6.aspx)]

Il controllo SiteMapDataSource restituisce il livello della gerarchia una mappa del sito alla volta, inizia con il nodo radice della mappa del sito (Home, in questo esempio), quindi al livello successivo (Basic Reporting, Filtering Reports e Customized Formatting) e così via. Quando si associa SiteMapDataSource a un controllo Repeater, viene enumerato il primo livello restituito e crea un'istanza di `ItemTemplate` per ogni `SiteMapNode` istanza in tale primo livello. Accedere a una particolare proprietà del `SiteMapNode`, è possibile usare `Eval(propertyName)`, che in modo da ottenere ciascuno `SiteMapNode`del `Url` e `Title` proprietà per il controllo collegamento ipertestuale.

L'esempio di Repeater precedente verrà eseguito il rendering di markup seguente:


[!code-html[Main](master-pages-and-site-navigation-cs/samples/sample7.html)]

Questi nodi della mappa del sito (Basic Reporting, Filtering Reports e Customized Formatting) costituiscono il *secondo* a livello della mappa del sito viene eseguito il rendering, non per la prima. Infatti, il controllo SiteMapDataSource `ShowStartingNode` proprietà è impostata su False, causando il controllo SiteMapDataSource ignorare il nodo radice della mappa del sito e iniziando invece la restituzione di secondo livello nella gerarchia della mappa del sito.

Per visualizzare gli elementi figlio per il Basic Reporting, Filtering Reports e Customized Formatting `SiteMapNode` s, possiamo aggiungere un altro controllo Repeater all'iniziale di Repeater `ItemTemplate`. Il secondo Repeater verrà associato il `SiteMapNode` dell'istanza `ChildNodes` proprietà, come illustrato di seguito:


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample8.aspx)]

Questi due controlli Repeater producono il markup seguente (alcuni tag sono stati rimossi per brevità):


[!code-html[Main](master-pages-and-site-navigation-cs/samples/sample9.html)]

Utilizzando gli stili CSS scelti dal [Rachel Andrew](http://www.rachelandrew.co.uk/)libro [The CSS Anthology: 101 Essential Tips, consigli, &amp; Hacks](https://www.amazon.com/gp/product/0957921888/qid=1137565739/sr=8-1/ref=pd_bbs_1/103-0562306-3386214?n=507846&amp;s=books&amp;v=glance), la `<ul>` e `<li>` elementi hanno ora uno stile in modo che i tag producano il seguente output di visualizzazione:


![Menu formato mediante due controlli Repeater e alcuni CSS](master-pages-and-site-navigation-cs/_static/image27.png)

**Figura 11**: un Menu composto da due controlli Repeater e alcuni CSS


Questo menu è nella pagina master e associato alla mappa del sito definita `Web.sitemap`, ovvero che qualsiasi modifica alla mappa del sito verrà riflessa immediatamente in tutte le pagine che utilizzano il `Site.master` pagina master.

## <a name="disabling-viewstate"></a>Disattivazione di ViewState

Tutti i controlli ASP.NET possono mantenere il proprio stato per il [lo stato di visualizzazione](https://msdn.microsoft.com/msdnmag/issues/03/02/CuttingEdge/), che viene serializzata come un campo del form nascosto nell'HTML sottoposto a rendering. Lo stato di visualizzazione viene utilizzato dai controlli ricordare il relativo stato modificato a livello di programmazione tramite i postback, ad esempio i dati associati a un controllo Web. Mentre lo stato di visualizzazione è sufficiente informazioni vengano memorizzate postback, esso aumenta le dimensioni dei tag devono essere inviati al client e può causare eccessivo contenuto delle pagine se non monitorato attentamente. I controlli Web dei dati in particolare GridView sono noti per aggiungere decine di kilobyte extra di tag a una pagina. Mentre tale incremento può essere irrilevante per gli utenti a banda larga o intranet, lo stato di visualizzazione è possibile aggiungere alcuni secondi per il round trip per gli utenti remoti.

Per verificare l'impatto di stato di visualizzazione, visitare una pagina in un browser e quindi visualizzare l'origine inviata dalla pagina web (in Internet Explorer, andare al menu View e scegliere l'opzione di origine). È anche possibile attivare [traccia delle pagine](https://msdn.microsoft.com/library/sfbfw58f.aspx) per visualizzare l'allocazione dello stato di visualizzazione utilizzato da ognuno dei controlli nella pagina. Le informazioni sullo stato di visualizzazione viene serializzate in un campo del form nascosto denominato `__VIEWSTATE`, che si trova in un `<div>` elemento subito dopo l'apertura `<form>` tag. Lo stato di visualizzazione viene mantenuto solo quando è presente un modulo Web utilizzato Se la pagina ASP.NET non include un' `<form runat="server">` nella relativa sintassi dichiarativa non avrà un `__VIEWSTATE` campo del form nascosto nel markup sottoposto a rendering.

Il `__VIEWSTATE` campo del form generato dalla pagina master aggiunge circa 1.800 byte per il markup della pagina generata. Questo eccesso è dovuto principalmente al controllo Repeater, come il contenuto del controllo SiteMapDataSource viene mantenuto nello stato di visualizzazione. Anche se 1.800 byte extra potrebbero non sembrare troppi, quando si usa un controllo GridView con molti campi e record, lo stato di visualizzazione può facilmente aumentare fino di un fattore pari o superiore a 10.

Lo stato di visualizzazione può essere disabilitato a livello di pagina o controllo impostando il `EnableViewState` proprietà `false`, in tal modo riduzione delle dimensioni di rendering dei tag. Poiché lo stato di visualizzazione per un Web i dati a controlli associati postback del controllo persistente di dati, quando lo stato di visualizzazione per una data di disabilitazione controllo Web i dati devono essere associati su ciascun postback. In ASP.NET versione 1.x la responsabilità di questa sulle spalle dei sviluppatore; con ASP.NET 2.0, tuttavia, i controlli Web dei dati vengono associati nuovamente al proprio controllo origine dati in ogni postback se necessario.

Per ridurre lo stato di visualizzazione della pagina è possibile impostare il controllo Repeater `EnableViewState` proprietà `false`. Questa operazione può essere eseguita nella finestra delle proprietà nella finestra di progettazione o in modo dichiarativo nella visualizzazione origine. Dopo aver apportato questa modifica markup dichiarativo del controllo Repeater dovrebbe essere simile:


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample10.aspx)]

Dopo il rendering di questa modifica, la pagina visualizzazione dimensioni dello stato ha si riducono a 52 byte, un risparmio del 97% nello stato di visualizzazione delle dimensioni. In questa serie di esercitazioni lo stato di visualizzazione dei dati di controlli Web verrà disattivato per impostazione predefinita per ridurre le dimensioni del rendering dei tag. Nella maggior parte degli esempi la `EnableViewState` verrà impostata su `false` e senza alcuna indicazione. L'unica volta verrà discusso lo stato di visualizzazione è in scenari in cui deve essere abilitato affinché i dati ai controlli Web per fornire le funzionalità previste.

## <a name="step-4-adding-breadcrumb-navigation"></a>Passaggio 4: Aggiunta dello spostamento Breadcrumb

Per completare la pagina master, è possibile aggiungere un elemento dell'interfaccia utente di spostamento breadcrumb a ogni pagina. Il breadcrumb indica rapidamente agli utenti la posizione corrente nella gerarchia del sito. Aggiungere un breadcrumb in ASP.NET 2.0 è facile aggiungere un controllo SiteMapPath alla pagina, non è necessario alcun codice.

Per il sito, aggiungere questo controllo per l'intestazione `<div>`:


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample11.aspx)]

Il breadcrumb indica la pagina corrente, l'utente sta visitando nella gerarchia della mappa del sito, nonché del sito "predecessori," del nodo della mappa tutti gli altri fino alla radice (Home, in questo esempio).


![Il Breadcrumb Visualizza la pagina corrente e i predecessori del sito eseguire il mapping di gerarchia](master-pages-and-site-navigation-cs/_static/image28.png)

**Figura 12**: il percorso di navigazione viene visualizzata la pagina corrente e i predecessori del sito eseguire il mapping di gerarchia


## <a name="step-5-adding-the-default-page-for-each-section"></a>Passaggio 5: Aggiunta della pagina predefinita per ogni sezione

Le esercitazioni nel sito sono suddivisi in categorie diverse Basic Reporting, Filtering, Custom formattazione, e così via con una cartella per ogni categoria e le corrispondenti esercitazioni come pagine ASP.NET all'interno della cartella. Ogni cartella contiene inoltre un `Default.aspx` pagina. Questa pagina predefinita, vengono visualizzate tutte le esercitazioni per la sezione corrente. Vale a dire, per il `Default.aspx` nella `BasicReporting` cartella sono disponibili collegamenti a `SimpleDisplay.aspx`, `DeclarativeParams.aspx`, e `ProgrammaticParams.aspx`. In questo caso, anche in questo caso, è possibile usare la `SiteMap` classe e un controllo Web per visualizzare le informazioni basate sulla mappa del sito di dati definiti in `Web.sitemap`.

È possibile visualizzare un elenco non ordinato usando un controllo Repeater, ma questa volta che verrà visualizzato il titolo e descrizione delle esercitazioni. Dal momento che il markup e codice per ottenere questo risultato devono essere ripetuti per ogni `Default.aspx` pagina, è possibile incapsulare questa logica dell'interfaccia utente in un [controllo utente](https://msdn.microsoft.com/library/y6wb1a0e.aspx). Creare una cartella nel sito Web denominata `UserControls` e aggiungervi un nuovo elemento di tipo controllo utente Web denominato `SectionLevelTutorialListing.ascx`e aggiungere il markup seguente:


[![Aggiungere un nuovo controllo utente Web alla cartella UserControls](master-pages-and-site-navigation-cs/_static/image30.png)](master-pages-and-site-navigation-cs/_static/image29.png)

**Figura 13**: aggiungere un nuovo controllo utente Web per il `UserControls` cartella ([fare clic per visualizzare l'immagine con dimensioni normali](master-pages-and-site-navigation-cs/_static/image31.png))


SectionLevelTutorialListing.ascx


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample12.aspx)]

SectionLevelTutorialListing.ascx.cs


[!code-csharp[Main](master-pages-and-site-navigation-cs/samples/sample13.cs)]

Nell'esempio precedente Repeater sono stati associati i `SiteMap` dati al controllo Repeater in modo dichiarativo; il `SectionLevelTutorialListing` controllo utente, invece, l'operazione a livello di codice. Nel `Page_Load` gestore eventi, viene effettuato un controllo per assicurarsi che questa pagina s URL esegue il mapping a un nodo nella mappa del sito. Se questo controllo utente viene usato in una pagina che non ha un corrispondente `<siteMapNode>` intervento `SiteMap.CurrentNode` restituirà `null` e nessun dato verrà associato a Repeater. Supponendo di avere una `CurrentNode`, viene eseguita l'associazione relativa `ChildNodes` insieme a Repeater. Dato che la mappa del sito è configurata in modo che il `Default.aspx` ogni sezione della pagina è il nodo padre di tutte le esercitazioni nella sezione, questo codice visualizza collegamenti a e le descrizioni di tutte le esercitazioni, come illustrato nella cattura di schermata seguente.

Dopo aver creato il controllo Repeater, aprire il `Default.aspx` pagine in ognuna delle cartelle, passare alla visualizzazione progettazione e trascinare semplicemente il controllo utente da Esplora soluzioni nell'area di progettazione in cui si desidera venga visualizzato l'elenco delle esercitazioni.


[![Il controllo utente è stato aggiunto a default. aspx](master-pages-and-site-navigation-cs/_static/image33.png)](master-pages-and-site-navigation-cs/_static/image32.png)

**Figura 14**: il controllo utente è stato aggiunto alla `Default.aspx` ([fare clic per visualizzare l'immagine con dimensioni normali](master-pages-and-site-navigation-cs/_static/image34.png))


[![Sono elencate le esercitazioni Basic Reporting](master-pages-and-site-navigation-cs/_static/image36.png)](master-pages-and-site-navigation-cs/_static/image35.png)

**Figura 15**: sono elencate esercitazioni Basic Reporting ([fare clic per visualizzare l'immagine con dimensioni normali](master-pages-and-site-navigation-cs/_static/image37.png))


## <a name="summary"></a>Riepilogo

Con la mappa del sito definita e la pagina master completa, è ora disponibile uno schema di layout e la navigazione delle pagine coerente per le esercitazioni relative ai dati. Indipendentemente dal numero di pagine aggiunte al nostro sito, aggiornare le informazioni di navigazione a livello di sito pagina layout o un sito è un processo semplice e rapido a causa di queste informazioni in corso centralizzate. In particolare, le informazioni sul layout di pagina è definite nella pagina master `Site.master` e il mappa del sito in `Web.sitemap`. Non è stato necessario scrivere *qualsiasi* codice per ottenere questo meccanismo di layout e la navigazione delle pagine nell'intero sito e vengono conservati WYSIWYG completo supporto di progettazione in Visual Studio.

Dopo aver completato il livello di accesso ai dati e il livello di logica di Business e di una pagina coerente layout ed esplorazione definita, siamo pronti per iniziare l'esplorazione dei modelli di report comuni. Nel [successivo](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) tre esercitazioni vengono esaminati le attività di reporting base visualizzazione dei dati recuperati da BLL di GridView, DetailsView e FormView.

Buona programmazione!

## <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Cenni preliminari sulle pagine Master ASP.NET](https://msdn.microsoft.com/library/wtxbf3hh.aspx)
- [Pagine master in ASP.NET 2.0](http://odetocode.com/Articles/419.aspx)
- [ASP.NET 2.0 Progettazione modelli](https://msdn.microsoft.com/asp.net/reference/design/templates/default.aspx)
- [Cenni preliminari sulla navigazione sito ASP.NET](https://msdn.microsoft.com/library/e468hxky.aspx)
- [Analisi di ASP.NET 2.0 di esplorazione del sito](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Funzionalità di spostamento di ASP.NET 2.0 sito](https://weblogs.asp.net/scottgu/archive/2005/11/20/431019.aspx)
- [Informazioni sullo stato di visualizzazione ASP.NET](https://msdn.microsoft.com/library/default.asp?url=/library/dnaspp/html/viewstate.asp)
- [Procedura: abilitare la traccia per una pagina ASP.NET](https://msdn.microsoft.com/library/94c55d08%28VS.80%29.aspx)
- [Controlli utente ASP.NET](https://msdn.microsoft.com/library/y6wb1a0e.aspx)

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. I revisori per questa esercitazione sono Liz Shulok, Dennis Patterson e Hilton Giesenow. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](creating-a-business-logic-layer-cs.md)
> [Successivo](creating-a-data-access-layer-vb.md)
