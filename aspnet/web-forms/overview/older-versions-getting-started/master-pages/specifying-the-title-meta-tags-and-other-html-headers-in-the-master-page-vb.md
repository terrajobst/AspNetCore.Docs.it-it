---
uid: web-forms/overview/older-versions-getting-started/master-pages/specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb
title: Specificazione di titolo, metatag e altre intestazioni HTML nella pagina Master (VB) | Microsoft Docs
author: rick-anderson
description: Esamina le diverse tecniche per la definizione assortite &lt;head&gt; gli elementi della pagina Master dalla pagina contenuto.
ms.author: riande
ms.date: 05/21/2008
ms.assetid: ea8196f5-039d-43ec-8447-8997ad4d3900
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb
msc.type: authoredcontent
ms.openlocfilehash: 1b28e6df0e0ab25e8292b6523c9ad7482301a511
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835362"
---
<a name="specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb"></a>Specificazione di titolo, metatag e altre intestazioni HTML nella pagina Master (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare il codice](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_03_VB.zip) o [Scarica il PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_03_VB.pdf)

> Esamina le diverse tecniche per la definizione assortite &lt;head&gt; gli elementi della pagina Master dalla pagina contenuto.


## <a name="introduction"></a>Introduzione

Dispongono di nuove pagine master create in Visual Studio 2008, per impostazione predefinita, i due controlli ContentPlaceHolder: uno denominato `head`e si trova nel `<head>` elemento; e uno denominato `ContentPlaceHolder1`, posizionato all'interno del Web Form. Lo scopo di `ContentPlaceHolder1` consiste nel definire un'area del modulo Web che possono essere personalizzati in base a una pagina per pagina. Il `head` ContentPlaceHolder consente alle pagine aggiungere contenuto personalizzato per il `<head>` sezione. (Naturalmente, questi elementi due ContentPlaceHolder possono essere modificati o rimossi e ContentPlaceHolder aggiuntive può essere aggiunto alla pagina master. Questa pagina master, `Site.master`, attualmente sono disponibili quattro controlli ContentPlaceHolder.)

Il codice HTML `<head>` elemento usata come repository per informazioni sul documento di pagina web che non fa parte del documento stesso. Sono incluse informazioni quali il titolo della pagina web, metainformazioni utilizzati dai motori di ricerca o crawler interni e collegamenti a risorse esterne, ad esempio i feed RSS, JavaScript e CSS i file. Alcune di queste informazioni potrebbero essere pertinenti a tutte le pagine nel sito Web. Potrebbe, ad esempio, si desidera importare globalmente le stesse regole CSS e file JavaScript per ogni pagina ASP.NET. Tuttavia, esistono parti del `<head>` elemento che sono specifici della pagina. Il titolo della pagina è un ottimo esempio.

In questa esercitazione si esamina come definire globali e specifiche della pagina `<head>` markup sezione nella pagina master e nelle relative pagine di contenuto.

## <a name="examining-the-master-pagesheadsection"></a>Analisi della pagina Master`<head>`sezione

Il file di paging master predefinito creato da Visual Studio 2008 contiene il markup seguente nel relativo `<head>` sezione:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample1.aspx)]

Si noti che il `<head>` elemento contiene un `runat="server"` attributo, che indica che si tratta di un controllo server (anziché il codice HTML statico). Tutte le pagine ASP.NET derivano dal [ `Page` classe](https://msdn.microsoft.com/library/system.web.ui.page.aspx), che si trova nel `System.Web.UI` dello spazio dei nomi. Questa classe contiene un [ `Header` proprietà](https://msdn.microsoft.com/library/system.web.ui.page.header.aspx) che consente di accedere alla pagina `<head>` area. Usando il `Header` proprietà, è possibile impostare il titolo della pagina ASP.NET o aggiungere tag aggiuntivi per il rendering `<head>` sezione. È possibile, quindi, per personalizzare una pagina di contenuto `<head>` elemento scrivendo un frammento di codice della pagina `Page_Load` gestore dell'evento. Esaminiamo come impostare a livello di codice il titolo della pagina nel passaggio 1.

Il markup visualizzato nei `<head>` elemento precedente include anche un controllo ContentPlaceHolder denominato `head`. Questo controllo ContentPlaceHolder non è necessario, come le pagine di contenuto è possono aggiungere contenuto personalizzato per il `<head>` elemento a livello di codice. È utile, tuttavia, nelle situazioni in cui deve aggiungere il markup statico a una pagina di contenuto di `<head>` elemento come il markup statico può essere aggiunto in modo dichiarativo ai corrispondenti controlli contenuto anziché a livello di codice.

Oltre al `<title>` elemento e `head` del ContentPlaceHolder, la pagina master `<head>` elemento dovrebbe contenere `<head>`-markup di livello che sono comuni a tutte le pagine. Nel nostro sito Web, tutte le pagine di usano le regole CSS definite nel `Styles.css` file. Di conseguenza, sono stati aggiornati i `<head>` elemento nel [ *creazione di un Layout a livello di sito con pagine Master* ](creating-a-site-wide-layout-using-master-pages-vb.md) esercitazione per includere un corrispondente `<link>` elemento. Nostri `Site.master` corrente della pagina master `<head>` markup è illustrato di seguito.


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample2.aspx)]

## <a name="step-1-setting-a-content-pages-title"></a>Passaggio 1: Impostazione del titolo di una pagina contenuto

Titolo della pagina web viene specificato tramite la `<title>` elemento. È importante impostare il titolo di ogni pagina su un valore appropriato. Quando si visita una pagina, il titolo viene visualizzato nella barra del titolo del browser. Inoltre, quando una pagina di segnalibri, i browser usano il titolo della pagina come il nome suggerito per il segnalibro. Inoltre, molti motori di ricerca mostrano il titolo della pagina quando si visualizzano i risultati della ricerca.

> [!NOTE]
> Per impostazione predefinita, Visual Studio imposta la `<title>` elemento nella pagina master per "Untitled Page". Analogamente, le nuove pagine ASP.NET sono loro `<title>` impostato su "Untitled Page", troppo. Poiché può essere facile dimenticare di impostare il titolo della pagina su un valore appropriato, esistono molte pagine su Internet con il titolo "Untitled Page". La ricerca di Google per le pagine web con questo titolo restituisce circa 2,460,000 risultati. Nemmeno Microsoft è soggetto alla pubblicazione di pagine web con il titolo "Untitled Page". Al momento della stesura di questo articolo, una ricerca di Google segnalato 236 tali pagine web nel dominio Microsoft.com.


Una pagina ASP.NET può specificare il titolo in uno dei modi seguenti:

- Inserendo il valore direttamente all'interno di `<title>` elemento
- Usando il `Title` attributo la `<%@ Page %>` (direttiva)
- Impostazione a livello di codice della pagina `Title` proprietà usando codice simile `Page.Title="title"` o `Page.Header.Title="title"`.

Contenuto di pagine non hanno un `<title>` elemento, perché è definito nella pagina master. Pertanto, per impostare il titolo di una pagina di contenuto è possibile usare la `<%@ Page %>` della direttiva `Title` attributo oppure impostarlo a livello di codice.

### <a name="setting-the-pages-title-declaratively"></a>Impostare il titolo della pagina in modo dichiarativo

Titolo della pagina di contenuto può essere impostato in modo dichiarativo mediante la `Title` attributo del [ `<%@ Page %>` direttiva](https://msdn.microsoft.com/library/ydy4x04a.aspx). Questa proprietà può essere impostata direttamente modificando il `<%@ Page %>` direttiva o tramite la finestra Proprietà. Diamo un'occhiata entrambi gli approcci.

Dalla visualizzazione origine, individuare il `<%@ Page %>` direttiva, che si trova nella parte superiore del markup dichiarativo della pagina. Il `<%@ Page %>` direttiva `Default.aspx` segue:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample3.aspx)]

Il `<%@ Page %>` direttiva specifica gli attributi specifici della pagina usati dal motore ASP.NET durante l'analisi e la compilazione della pagina. Ciò include il file pagina master, la posizione del suo file di codice e il relativo titolo, tra le altre informazioni.

Per impostazione predefinita, quando si crea una nuova pagina di contenuto Visual Studio imposta la `Title` dell'attributo "Untitled Page". Change `Default.aspx`del `Title` attributo "Untitled Page" a "Esercitazioni di pagina Master" e quindi visualizzare la pagina tramite un browser. Figura 1 mostra la barra del titolo del browser che riflette il nuovo titolo della pagina.


![Barra del titolo del Browser Mostra ora &quot;esercitazioni pagina Master&quot; invece di &quot;Untitled Page&quot;](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image1.png)

**Figura 01**: barra del titolo del Browser Mostra ora "Esercitazioni su pagina Master" anziché "Untitled Page"


Il titolo della pagina può essere impostato anche dalla finestra Proprietà. Dalla finestra delle proprietà, selezionare questa opzione documento nell'elenco a discesa per le proprietà di carico a livello di pagina, che include il `Title` proprietà. Figura 2 mostra la finestra delle proprietà dopo `Title` è stata impostata su "Esercitazioni su pagina Master".


![È possibile configurare anche il titolo dalla finestra delle proprietà,](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image2.png)

**Figura 02**: È possibile configurare anche il titolo dalla finestra delle proprietà,


### <a name="setting-the-pages-title-programmatically"></a>Impostare il titolo della pagina a livello di codice

La pagina master `<head runat="server">` markup viene convertito in un [ `HtmlHead` classe](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlhead.aspx) istanza quando viene eseguito il rendering della pagina dal motore di ASP.NET. Il `HtmlHead` classe ha un [ `Title` proprietà](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlhead.title.aspx) il cui valore viene riflessa nell'oggetto sottoposto a rendering `<title>` elemento. Questa proprietà è accessibile dalla classe code-behind della pagina ASP.NET tramite `Page.Header.Title`; queste stesse proprietà è accessibile anche tramite `Page.Title`.

Provare a impostare il titolo della pagina a livello di codice, passare al `About.aspx` code-behind della pagina di classi e creare un gestore eventi per la pagina `Load` evento. Quindi, impostare il titolo della pagina "esercitazioni di pagina Master:: sulla:: *data*", dove *data* è la data corrente. Dopo aver aggiunto questo codice di `Page_Load` gestore dell'evento dovrebbe essere simile al seguente:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample4.vb)]

Figura 3 Mostra barra del titolo del browser quando si visita il `About.aspx` pagina.


![Il titolo della pagina è impostata a livello di codice che include la data corrente](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image3.png)

**Figura 03**: titolo della pagina di è impostato a livello di codice che include la data corrente


## <a name="step-2-automatically-assigning-a-page-title"></a>Passaggio 2: Assegnare automaticamente un titolo della pagina

Come abbiamo visto nel passaggio 1, il titolo della pagina può essere impostato in modo dichiarativo o a livello di codice. Se si dimentica di modificare in modo esplicito il titolo per renderlo più descrittivo, tuttavia, la pagina avrà il titolo predefinito, "Untitled Page". In teoria, il titolo della pagina impostato automaticamente per Stati Uniti nel caso in cui non viene specificato in modo esplicito il relativo valore. Ad esempio, se in fase di esecuzione il titolo della pagina è "Untitled Page", si potrebbe essere opportuno avere il titolo aggiornato automaticamente per corrispondere al nome del file della pagina ASP.NET. La buona notizia è che con un po' di lavoro iniziale, che è possibile avere il titolo assegnato automaticamente.

Tutte le pagine web ASP.NET derivano dal `Page` classe nello spazio dei nomi System.Web.UI. Il `Page` classe definisce le funzionalità minime necessarie per una pagina ASP.NET ed espone le proprietà fondamentali, ad esempio `IsPostBack`, `IsValid`, `Request`, e `Response`, tra le molte altre. Spesso, ogni pagina in un'applicazione web richiede funzionalità aggiuntive o funzionalità. Un modo comune di fornire ciò consiste nel creare una classe di base di pagine personalizzate. Una classe di base di pagine personalizzate è una classe crei da cui deriva il `Page` classe e include funzionalità aggiuntive. Dopo aver creata questa classe di base, è possibile avere le pagine ASP.NET derivano da essa (anziché il `Page` classe) in modo che offre le funzionalità estese alle pagine ASP.NET.

In questo passaggio viene creata una pagina di base che imposta automaticamente il titolo della pagina al nome file della pagina ASP.NET, se il titolo non è in caso contrario, stato impostato in modo esplicito. Passaggio 3 vengono esaminati impostando il titolo della pagina basato sulla mappa del sito.

> [!NOTE]
> Un esame approfondito della creazione e utilizzo di classi base della pagina personalizzata esula dall'ambito di questa serie di esercitazioni. Per altre informazioni, leggere [utilizzando una classe di Base personalizzata per le classi di Code-Behind Your alla pagina ASP.NET](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx).


### <a name="creating-the-base-page-class"></a>Creazione della classe della pagina Base

La prima attività consiste nel creare una classe di base delle pagine, che è una classe che estende il `Page` classe. Iniziare aggiungendo un `App_Code` cartella al progetto facendo clic sul nome del progetto in Esplora soluzioni, scegliendo Aggiungi cartella ASP.NET e quindi selezionando `App_Code`. Successivamente, fare clic sui `App_Code` cartella e aggiungere una nuova classe denominata `BasePage.vb`. La figura 4 illustra Esplora soluzioni dopo la `App_Code` cartella e `BasePage.vb` classe sono state aggiunte.


![Aggiungere una cartella App_Code e una classe denominata BasePage](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image4.png)

**Figura 04**: aggiungere un `App_Code` cartella e una classe denominata `BasePage`


> [!NOTE]
> Visual Studio supporta due modalità di gestione del progetto: progetti di siti Web e progetti di applicazione Web. Il `App_Code` cartella è progettata per essere usato con il modello di progetto di sito Web. Se si usa il modello di progetto di applicazione Web, inserire il `BasePage.vb` classe in una cartella denominata qualcosa di diverso da `App_Code`, ad esempio `Classes`. Per altre informazioni su questo argomento, consultare [migrazione di un progetto sito Web a un progetto di applicazione Web](http://webproject.scottgu.com/VisualBasic/Migration2/Migration2.aspx).


Poiché la pagina di base personalizzata funge da classe base per classi ASP.NET alla pagina code-behind, è necessario estendere il `Page` classe.


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample5.vb)]

Ogni volta che viene richiesta una pagina ASP.NET procede attraverso una serie di fasi, che si conclude con la pagina richiesta viene eseguito il rendering in HTML. È possibile toccare in una fase eseguendo l'override di `Page` della classe `OnEvent` (metodo). Per la nostra base della pagina è possibile impostare automaticamente il titolo, se è non stato specificato in modo esplicito dal `LoadComplete` fase (che, come può immaginare, si verifica dopo il `Load` fase).

A tale scopo, eseguire l'override di `OnLoadComplete` (metodo) e immettere il codice seguente:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample6.vb)]

Il `OnLoadComplete` determinando se il metodo inizia al `Title` proprietà non è ancora stata impostata in modo esplicito. Se il `Title` è di proprietà `Nothing`, una stringa vuota, o con il valore "Untitled Page", viene assegnato al nome del file della pagina ASP.NET richiesta. Il percorso fisico per la pagina ASP.NET richiesta - `C:\MySites\Tutorial03\Login.aspx`, ad esempio, sono accessibili tramite il `Request.PhysicalPath` proprietà. Il `Path.GetFileNameWithoutExtension` viene usato il metodo per estrarre solo la parte del nome file e il nome del file viene quindi assegnato al `Page.Title` proprietà.

> [!NOTE]
> Utenti sono invitati a migliorare questa logica per migliorare il formato del titolo. Ad esempio, se il nome file della pagina è `Company-Products.aspx`, il codice precedente produce il titolo "Prodotti della società", ma in teoria il trattino verrebbe sostituito con uno spazio, come in "Prodotti aziendali". Inoltre, è consigliabile aggiungere uno spazio ogni volta che viene apportata una modifica di maiuscole. Vale a dire, è consigliabile aggiungere codice che trasforma il nome del file `OurBusinessHours.aspx` con un titolo di "gli orari di ufficio".


### <a name="having-the-content-pages-inherit-the-base-page-class"></a>Con le pagine contenute ereditano la classe di pagina Base

È ora necessario aggiornare le pagine ASP.NET nel sito per derivare dalla pagina base personalizzata (`BasePage`) anziché il `Page` classe. A tale scopo, passare a ogni classe code-behind e modificare la dichiarazione di classe da:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample7.vb)]

A:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample8.vb)]

Al termine dell'operazione, visitare il sito tramite un browser. Se si visita una pagina il cui titolo è impostato esplicitamente, ad esempio `Default.aspx` o `About.aspx`, viene usato il titolo specificato in modo esplicito. Se, tuttavia, si visita una pagina il cui titolo non è stato modificato da quello predefinito ("Untitled Page"), la classe di base di pagine imposta il titolo al nome file della pagina.

Figura 5 mostra il `MultipleContentPlaceHolders.aspx` pagina quando viene visualizzato tramite un browser. Si noti che il titolo è precisamente nome file della pagina (meno l'estensione), "MultipleContentPlaceHolders".


[![Se non è specificato in modo esplicito un titolo, nome file della pagina viene usato automaticamente](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image6.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image5.png)

**Figura 05**: se non è specificato in modo esplicito un titolo, nome file della pagina viene usato automaticamente ([fare clic per visualizzare l'immagine con dimensioni normali](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image7.png))


## <a name="step-3-basing-the-page-title-on-the-site-map"></a>Passaggio 3: Basare il titolo della pagina sulla mappa del sito

ASP.NET offre un framework di mapping sito affidabile che consente agli sviluppatori di pagina definire una mappa del sito gerarchica in una risorsa esterna (ad esempio una tabella di database o file XML) insieme ai controlli Web per la visualizzazione di informazioni relative alla mappa del sito (ad esempio, il controllo SiteMapPath, Menu e controlli TreeView).

Struttura della mappa del sito è possibile accedere anche a livello di codice da classe code-behind della pagina ASP.NET. In questo modo è possibile impostare automaticamente il titolo della pagina per il titolo del nodo corrispondente nella mappa del sito. È possibile migliorare il `BasePage` classe creata nel passaggio 2 in modo che offre questa funzionalità. Ma prima di tutto è necessario creare una mappa del sito per il sito.

> [!NOTE]
> Questa esercitazione si presuppone che il lettore abbia già familiarità con ASP. Funzionalità della mappa del sito di rete. Per altre informazioni sull'uso della mappa del sito, consultare la serie di articoli multiparte, [esaminando ASP. Esplorazione del sito di rete](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx).


### <a name="creating-the-site-map"></a>Creazione della mappa del sito

Il sistema del sito viene compilato sopra il [modello di provider](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), che separa la mappa del sito API dalla logica che serializza le informazioni sulla mappa del sito tra memoria e un archivio permanente. .NET Framework viene fornito con il [ `XmlSiteMapProvider` classe](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx), ovvero il provider di mappa del sito predefinito. Come suggerisce il nome, `XmlSiteMapProvider` utilizza un file XML come archivio di mappa del sito. È possibile utilizzare questo provider per definire la mappa del sito.

Iniziare creando un file di mappa del sito nella cartella radice del sito Web denominato `Web.sitemap`. A tale scopo, fare doppio clic sul nome del sito Web in Esplora soluzioni, scegliere Aggiungi nuovo elemento e selezionare il modello di mappa del sito. Assicurarsi che il file è denominato `Web.sitemap` e fare clic su Aggiungi.


[![Aggiungere un File denominato Web. sitemap alla cartella radice del sito Web](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image9.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image8.png)

**Figura 06**: aggiungere un File denominato `Web.sitemap` alla cartella radice del sito Web ([fare clic per visualizzare l'immagine con dimensioni normali](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image10.png))


Aggiungere il seguente codice XML per il `Web.sitemap` file:


[!code-xml[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample9.xml)]

Questo XML definisce la struttura della mappa del sito gerarchica illustrata nella figura 7.


![Mappa del sito è attualmente composta di tre nodi mappa del sito](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image11.png)

**Figura 07**: la mappa del sito è attualmente composta di tre nodi mappa del sito


Struttura della mappa del sito in esercitazioni future verrà aggiornata man mano che si aggiungono nuovi esempi.

### <a name="updating-the-master-page-to-include-navigation-web-controls"></a>L'aggiornamento della pagina Master per includere i controlli Web di navigazione

Ora che abbiamo definita una mappa del sito, è possibile aggiornare la pagina master per includere i controlli Web di navigazione. In particolare, è possibile aggiungere un controllo ListView nella colonna sinistra della sezione di lezioni che esegue il rendering di un elenco non ordinato con un elemento di elenco per ogni nodo definito nella mappa del sito.

> [!NOTE]
> Il controllo ListView è nuovo in ASP.NET versione 3.5. Se si usa una versione precedente di ASP.NET, usare invece il controllo Repeater. Per altre informazioni sul controllo ListView, vedere [ListView usando ASP.NET 3.5 e i controlli DataPager](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx).


Iniziare rimuovendo il markup di elenco non ordinato esistente dalla sezione lezioni. Successivamente, trascinare un controllo ListView dalla casella degli strumenti e rilasciarlo di sotto le lezioni sull'intestazione. Il ListView si trova nella sezione dati della casella degli strumenti, insieme ad altri controlli di visualizzazione: il controllo GridView, DetailsView e FormView. Impostare il ListView `ID` proprietà `LessonsList`.

Scegliere la configurazione guidata origine dati per associare il ListView per un nuovo controllo SiteMapDataSource denominato `LessonsDataSource`. Il controllo SiteMapDataSource restituisce la struttura gerarchica dal sistema di mappa del sito.


[![Associare un controllo SiteMapDataSource al controllo ListView LessonsList](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image13.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image12.png)

**Figura 08**: associare un controllo SiteMapDataSource al controllo ListView LessonsList ([fare clic per visualizzare l'immagine con dimensioni normali](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image14.png))


Dopo aver creato il controllo SiteMapDataSource, è necessario definire i modelli di ListView, in modo che esegue il rendering di un elenco non ordinato con un elemento di elenco per ogni nodo restituito dal controllo SiteMapDataSource. Ciò può essere eseguita usando il seguente markup del modello:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample10.aspx)]

Il `LayoutTemplate` genera il markup per un elenco non ordinato (`<ul>...</ul>`) mentre le `ItemTemplate` esegue il rendering di ogni elemento restituito da SiteMapDataSource come un elemento di elenco (`<li>`) che contiene un collegamento alla lezione particolare.

Dopo aver configurato i modelli di ListView, visitare il sito Web. Come illustrato nella figura 9, la sezione lezioni contiene un singolo elemento puntato, home page. Dove si trovano le informazioni e l'utilizzo di lezioni controlli ContentPlaceHolder più? Il controllo SiteMapDataSource è progettato per restituire un set di dati gerarchico, ma il controllo ListView può visualizzare solo un singolo livello della gerarchia. Di conseguenza, viene visualizzato solo il primo livello dei nodi della mappa del sito restituiti da SiteMapDataSource.


[![La sezione lezioni contiene un singolo elemento di elenco](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image16.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image15.png)

**Figura 09**: la sezione lezioni contiene un singolo elemento di elenco ([fare clic per visualizzare l'immagine con dimensioni normali](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image17.png))


Per visualizzare più livelli è possibile annidare ListView più all'interno di `ItemTemplate`. Questa tecnica è stata esaminata nel [ *pagine Master e spostamento nel sito* esercitazione](../../data-access/introduction/master-pages-and-site-navigation-vb.md) del mio [utilizzo di serie di esercitazioni dati](../../data-access/index.md). Tuttavia, per questa serie di esercitazioni la mappa del sito conterrà solo un due livelli: Home (il livello principale); e ogni lezione come elemento figlio della home page. Invece di creazione di un ListView annidati, possiamo invece indichiamo SiteMapDataSource per non restituire il nodo iniziale tramite l'impostazione relativa [ `ShowStartingNode` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemapdatasource.showstartingnode.aspx) a `False`. Il risultato finale è che il controllo SiteMapDataSource inizia con la restituzione di secondo livello di nodi della mappa del sito.

Con questa modifica, il ListView Visualizza gli elementi di punto elenco per le informazioni su e utilizzo di più controlli ContentPlaceHolder lezioni, ma omette un elemento punto elenco per la home page. Per risolvere questo problema, è possibile aggiungere un elemento di elenco puntato in modo esplicito per la casa nel `LayoutTemplate`:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample11.aspx)]

Configurare il controllo SiteMapDataSource per omettere il nodo di inizio e in modo esplicito aggiungendo un elemento di elenco puntato Home, la sezione lezioni ora visualizza l'output previsto.


[![La sezione lezioni contiene un elemento di elenco puntato per ogni nodo figlio e Home](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image19.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image18.png)

**Figura 10**: la sezione lezioni contiene un elemento di elenco puntato per ogni nodo figlio e la home page ([fare clic per visualizzare l'immagine con dimensioni normali](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image20.png))


### <a name="setting-the-title-based-on-the-site-map"></a>Impostazione del titolo basato sulla mappa del sito

Con la mappa del sito posto, è possibile aggiornare il `BasePage` classe da utilizzare il titolo specificato nella mappa del sito. Come è stato fatto nel passaggio 2, si vuole solo usare titolo del nodo della mappa del sito se il titolo della pagina non è stato impostato in modo esplicito dallo sviluppatore della pagina. Se la pagina richiesta non ha impostato in modo esplicito titolo della pagina e non viene trovato nella mappa del sito, quindi si tornerà alla con nome file della pagina richiesto (meno l'estensione), come è stato fatto nel passaggio 2. La figura 11 illustra questo processo decisionale.


![In assenza un esplicitamente impostato del titolo della pagina, viene usato il titolo del nodo della mappa del sito corrispondente](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image21.png)

**Figura 11**: In assenza un esplicitamente impostato del titolo della pagina, viene usato il titolo del nodo della mappa del sito corrispondente


Aggiorna il `BasePage` della classe `OnLoadComplete` metodo per includere il codice seguente:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample12.vb)]

Come in precedenza, il `OnLoadComplete` metodo inizia a determinando se il titolo della pagina è stato impostato in modo esplicito. Se `Page.Title` viene `Nothing`, una stringa vuota, o viene assegnato il valore "Untitled Page", quindi il codice assegna automaticamente un valore a `Page.Title`.

Per determinare il titolo da utilizzare, il codice inizia facendo il [ `SiteMap` classe](https://msdn.microsoft.com/library/system.web.sitemap.aspx)del [ `CurrentNode` proprietà](https://msdn.microsoft.com/library/system.web.sitemap.currentnode.aspx). `CurrentNode` Restituisce il [ `SiteMapNode` ](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx) istanza nella mappa del sito che corrisponde alla pagina attualmente richiesta. Supponendo che la pagina attualmente richiesta viene trovato all'interno della mappa del sito, il `SiteMapNode`del `Title` proprietà è assegnata al titolo della pagina. Se la pagina attualmente richiesta non è presente nella mappa del sito `CurrentNode` restituisce `Nothing` e nome file della pagina richiesto viene usato come titolo (come è stato fatto nel passaggio 2).

Figura 12 vengono illustrate le `MultipleContentPlaceHolders.aspx` pagina quando viene visualizzato tramite un browser. Poiché il titolo della pagina non è impostato in modo esplicito, viene usato il titolo del nodo della mappa del sito corrispondente.


![Titolo della pagina MultipleContentPlaceHolders.aspx viene effettuato il pull dalla mappa del sito](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image22.png)

**Figura 12**: titolo della pagina MultipleContentPlaceHolders.aspx The viene effettuato il pull dalla mappa del sito


## <a name="step-4-adding-other-page-specific-markup-to-theheadsection"></a>Passaggio 4: Aggiunta di altri Markup specifico della pagina per il`<head>`sezione

I passaggi 1, 2 e 3 preso in esame la personalizzazione di `<title>` elemento in base a una pagina per pagina. Oltre a `<title>`, il `<head>` sezione può contenere `<meta>` elementi e `<link>` elementi. Come indicato in precedenza in questa esercitazione `Site.master`del `<head>` sezione sono inclusi un `<link>` elemento `Styles.css`. Perché ciò `<link>` elemento è definito all'interno della pagina master, è incluso nel `<head>` sezione per tutte le pagine di contenuto. Ora come si vedrà aggiunta `<meta>` e `<link>` elementi in base a una pagina dalla pagina?

Il modo più semplice per aggiungere contenuto specifico della pagina per il `<head>` sezione consiste nel creare un controllo ContentPlaceHolder nella pagina master. Si dispone già di questo tipo un ContentPlaceHolder (denominato `head`). Pertanto, per aggiungere custom `<head>` markup, creare un oggetto corrispondente controllo nella pagina di contenuto e inserirvi il markup.

Per illustrare personalizzata aggiunta `<head>` markup a una pagina, è possibile includere un `<meta>` description (elemento) per la serie di pagine di contenuto. Il `<meta>` description (elemento) fornisce una breve descrizione della pagina web, la maggior parte dei motori di ricerca incorporano le informazioni in una determinata forma quando si visualizzano i risultati della ricerca.

Oggetto `<meta>` description (elemento) ha il formato seguente:


[!code-html[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample13.html)]

Per aggiungere il markup per una pagina di contenuto, aggiungere il testo precedente nel controllo contenuto che viene eseguito il mapping della pagina master `head` ContentPlaceHolder. Ad esempio, per definire un `<meta>` description (elemento) per `Default.aspx`, aggiungere il markup seguente:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample14.aspx)]

Poiché il `head` ContentPlaceHolder non è presente all'interno del corpo della pagina HTML, il codice aggiunto al controllo contenuto non viene visualizzato nella visualizzazione progettazione. Per vedere le `<meta>` descrizione elemento visita `Default.aspx` tramite un browser. Dopo la pagina è stata caricata, visualizzare l'origine e si noti che il `<head>` sezione sono inclusi il markup specificato nel controllo contenuto.

Si consiglia di aggiungere `<meta>` elementi description `About.aspx`, `MultipleContentPlaceHolders.aspx`, e `Login.aspx`.

### <a name="programmatically-adding-markup-to-theheadregion"></a>Aggiunta a livello di codice di Markup per il`<head>`area

Il `head` ContentPlaceHolder ci consente di aggiungere in modo dichiarativo markup personalizzata della pagina master `<head>` area. Inoltre è possibile aggiungere tag personalizzati a livello di codice. Si tenga presente che il `Page` della classe `Header` proprietà restituisce il `HtmlHead` istanza definita nella pagina master (il `<head runat="server">`).

La possibilità di aggiungere a livello di codice contenuto per il `<head>` area è utile quando il contenuto da aggiungere è dinamico. Ad esempio si basa sull'utente potrebbe visitare la pagina. forse vengono recuperato da un database. Indipendentemente dal motivo, è possibile aggiungere contenuto al `HtmlHead` mediante l'aggiunta di controlli al relativo `Controls` insieme come segue:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample15.vb)]

Il codice sopra riportato aggiunge il `<meta>` elemento parole chiave per il `<head>` area, che fornisce un elenco delimitato da virgole di parole chiave che descrivono la pagina. Si noti che per aggiungere un `<meta>` tag si crea un' [ `HtmlMeta` ](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlmeta.aspx) dell'istanza, impostare relativo `Name` e `Content` proprietà e quindi aggiungerlo al `Header`del `Controls` insieme. Analogamente, a livello di codice aggiungere un `<link>` elemento, creare un [ `HtmlLink` ](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmllink.aspx) dell'oggetto, impostarne le proprietà e quindi aggiungerlo al `Header`del `Controls` raccolta.

> [!NOTE]
> Per aggiungere un codice arbitrario, creare un [ `LiteralControl` ](https://msdn.microsoft.com/library/system.web.ui.literalcontrol.aspx) dell'istanza, impostare relativo `Text` proprietà e quindi aggiungerlo al `Header`del `Controls` raccolta.


## <a name="summary"></a>Riepilogo

In questa esercitazione è stato in diversi modi per aggiungere `<head>` markup area in base a una pagina per pagina. Una pagina master deve includere un' `HtmlHead` istanza (`<head runat="server">`) con un controllo ContentPlaceHolder. Il `HtmlHead` istanza consente le pagine di contenuto per l'accesso a livello di codice le `<head>` area e in modo dichiarativo e a livello di codice della pagina di impostare il titolo del; del controllo ContentPlaceHolder consente di markup personalizzata da aggiungere al `<head>` sezione in modo dichiarativo tramite un controllo contenuto.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Impostazione in modo dinamico il titolo della pagina in ASP.NET](http://aspnet.4guysfromrolla.com/articles/051006-1.aspx)
- [Esame di ASP. Navigazione nel sito di rete](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Come usare metatag HTML](http://searchenginewatch.com/showPage.html?page=2167931)
- [Pagine master in ASP.NET](http://www.odetocode.com/articles/419.aspx)
- [Utilizzo di ASP.NET 3.5 ListView e controlli DataPager](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)
- [Usando una classe di Base personalizzata per le classi di ASP.NET alla pagina Code-Behind](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx)

### <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di più libri e fondatore di 4GuysFromRolla.com, ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach manualmente ASP.NET 3.5 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo Scott [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) o il suo blog all'indirizzo [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. I revisori per questa esercitazione sono state Zack Jones e Suchi Banerjee. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Precedente](multiple-contentplaceholders-and-default-content-vb.md)
> [Successivo](urls-in-master-pages-vb.md)
