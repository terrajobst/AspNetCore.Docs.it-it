---
uid: web-forms/overview/older-versions-getting-started/master-pages/specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs
title: Specifica il titolo, tag Meta e altre intestazioni HTML della pagina Master (c#) | Documenti Microsoft
author: rick-anderson
description: Analizza diverse tecniche per la definizione di diverse &lt;head&gt; gli elementi della pagina Master dalla pagina contenuto.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2008
ms.topic: article
ms.assetid: 0aa1c84f-c9e2-4699-b009-0e28643ecbc6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs
msc.type: authoredcontent
ms.openlocfilehash: 30324c45fd8acbcba43808307512ef7aecffe695
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-c"></a>Specifica il titolo, tag Meta e altre intestazioni HTML della pagina Master (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare codice](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_03_CS.zip) o [Scarica il PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_03_CS.pdf)

> Analizza diverse tecniche per la definizione di diverse &lt;head&gt; gli elementi della pagina Master dalla pagina contenuto.


## <a name="introduction"></a>Introduzione

Dispongono di nuove pagine master create in Visual Studio 2008, per impostazione predefinita, i due controlli ContentPlaceHolder: uno denominato head e si trova nel `<head>` elemento; e uno denominato `ContentPlaceHolder1`, posizionato all'interno del Web Form. Lo scopo di `ContentPlaceHolder1` consiste nel definire un'area del modulo Web che possono essere personalizzati in base a una pagina per pagina. Il `head` ContentPlaceHolder consente di aggiungere contenuto personalizzato per le pagine di `<head>` sezione. (Naturalmente, queste due ContentPlaceHolder può essere modificato o rimossa e ContentPlaceHolder aggiuntive possono essere aggiunti della pagina master. La pagina master, `Site.master`, attualmente dispone di quattro controlli ContentPlaceHolder.)

Il codice HTML `<head>` elemento funge da repository per informazioni sul documento di pagina web che non fa parte del documento stesso. Sono incluse informazioni quali il titolo della pagina web, metainformazioni file utilizzati di motori di ricerca o crawler interno e collegamenti a risorse esterne, ad esempio i feed RSS, JavaScript e CSS. Alcune di queste informazioni potrebbero essere pertinenti a tutte le pagine nel sito Web. Potrebbe ad esempio, si desidera importare globalmente le stesse regole CSS e i file di JavaScript per ogni pagina ASP.NET. Tuttavia, vi sono parti del `<head>` elemento che sono specifici della pagina. Il titolo della pagina è riportato un esempio principale.

In questa esercitazione è esaminare come definire globale e specifico della pagina `<head>` markup sezione nella pagina master e nelle relative pagine di contenuto.

## <a name="examining-the-master-pagesheadsection"></a>Analisi della pagina Master`<head>`sezione

Il file di pagina master predefinita creato da Visual Studio 2008 contiene il markup seguente nel relativo `<head>` sezione:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample1.aspx)]

Si noti che il `<head>` elemento contiene un `runat="server"` attributo, che indica che si tratta di un controllo server (anziché HTML statico). Tutte le pagine ASP.NET derivano il [ `Page` classe](https://msdn.microsoft.com/library/system.web.ui.page.aspx), che si trova nel `System.Web.UI` dello spazio dei nomi. Questa classe contiene un `Header` proprietà che fornisce l'accesso alla pagina `<head>` area. Utilizzo di [ `Header` proprietà](https://msdn.microsoft.com/library/system.web.ui.page.header.aspx) è possibile impostare il titolo della pagina ASP.NET o aggiungere il markup aggiuntive per il rendering `<head>` sezione. È quindi possibile personalizzare una pagina di contenuto `<head>` elemento mediante la scrittura di codice della pagina `Page_Load` gestore dell'evento. Verrà esaminato come impostare a livello di codice il titolo della pagina nel passaggio 1.

Il markup di `<head>` elemento precedente include inoltre un controllo ContentPlaceHolder denominato head. Questo controllo ContentPlaceHolder non è necessario, come le pagine di contenuto è possono aggiungere contenuto personalizzato per il `<head>` elemento a livello di codice. È utile, tuttavia, in situazioni in cui una pagina di contenuto deve aggiungere il markup statico per il `<head>` elemento come il markup statico può essere aggiunto in modo dichiarativo il controllo contenuto corrispondente anziché a livello di codice.

Oltre al `<title>` dell'elemento e head ContentPlaceHolder, la pagina master `<head>` elemento deve contenere `<head>`-markup livello che sono comuni a tutte le pagine. In questo sito Web, tutte le pagine di utilizzano le regole CSS definite nel `Styles.css` file. Di conseguenza, è stato aggiornato il `<head>` elemento il [ *creazione di un Layout a livello di sito con pagine Master* ](creating-a-site-wide-layout-using-master-pages-cs.md) esercitazione per includere un corrispondente `<link>` elemento. Il nostro `Site.master` corrente della pagina master `<head>` markup è illustrato di seguito.


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample2.aspx)]

## <a name="step-1-setting-a-content-pages-title"></a>Passaggio 1: Impostazione del titolo della pagina contenuto

Titolo della pagina web viene specificato tramite il `<title>` elemento. È importante impostare il titolo di ogni pagina su un valore appropriato. Quando si visita una pagina, il titolo viene visualizzato nella barra del titolo del browser. Inoltre, quando una pagina di segnalibri, i browser usano il titolo della pagina come nome suggerito per il segnalibro. Inoltre, molti motori di ricerca mostrano il titolo della pagina quando si visualizzano i risultati della ricerca.

> [!NOTE]
> Per impostazione predefinita, Visual Studio imposta la `<title>` elemento nella pagina master "Senza nome pagina". Analogamente, le nuove pagine ASP.NET dispongono loro `<title>` impostato su "Senza nome pagina" troppo. Poiché può essere facile dimenticare di impostare il titolo della pagina su un valore appropriato, esistono molte pagine su Internet con il titolo "Senza nome pagina". Ricerca di Google per le pagine web con questo titolo restituiti approssimativamente 2,460,000 risultati. Anche Microsoft sono soggetti a pubblicazione di pagine web con il titolo "Senza nome pagina". Al momento della redazione del presente documento, una ricerca di Google segnalato 236 tali pagine web nel dominio Microsoft.com.


Una pagina ASP.NET è possibile specificare il titolo in uno dei modi seguenti:

- Inserendo il valore direttamente all'interno di `<title>` elemento
- Utilizzando il `Title` attributo il `<%@ Page %>` (direttiva)
- Impostazione a livello di codice della pagina `Title` proprietà utilizzando codice simile `Page.Title="title"` o `Page.Header.Title="title"`.

Le pagine non sono contenuto un `<title>` è definito l'elemento, come la pagina master. Pertanto, per impostare il titolo della pagina di contenuto è possibile utilizzare il `<%@ Page %>` della direttiva `Title` attributo o livello di codice.

### <a name="setting-the-pages-title-declaratively"></a>Impostazione in modo dichiarativo il titolo della pagina

Titolo della pagina di contenuto può essere impostata in modo dichiarativo mediante la `Title` attributo del [ `<%@ Page %>` direttiva](https://msdn.microsoft.com/library/ydy4x04a.aspx). Questa proprietà può essere impostata modificando direttamente il `<%@ Page %>` direttiva o tramite la finestra Proprietà. Diamo un'occhiata entrambi gli approcci.

Dalla visualizzazione origine, individuare il `<%@ Page %>` direttiva, nella parte superiore del markup dichiarativo della pagina. Il `<%@ Page %>` direttiva `Default.aspx` segue:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample3.aspx)]

Il `<%@ Page %>` direttiva specifica gli attributi specifici di pagina utilizzati dal motore di ASP.NET durante l'analisi e compilazione della pagina. Ciò include il file pagina master, la posizione di un file di codice e il relativo titolo, tra le altre informazioni.

Per impostazione predefinita, quando si crea una nuova pagina di contenuto Visual Studio imposta la `Title` attributo alla pagina senza nome. Modifica `Default.aspx`del `Title` dell'attributo da "Senza nome pagina" a "Esercitazioni su pagina Master" e quindi visualizzare la pagina tramite un browser. Figura 1 mostra la barra del titolo del browser che riflette il nuovo titolo della pagina.


![Barra del titolo del Browser verrà visualizzato &quot;esercitazioni pagina Master&quot; anziché &quot;senza titolo&quot;](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image1.png)

**Figura 01**: barra del titolo del Browser verrà visualizzato "Esercitazioni su pagina Master" anziché "Senza nome pagina"


Il titolo della pagina può essere impostato anche dalla finestra delle proprietà. Dalla finestra delle proprietà, selezionare documento nell'elenco a discesa per la proprietà di carico a livello di pagina, che include il `Title` proprietà. Figura 2 mostra la finestra Proprietà dopo `Title` è stata impostata su "Esercitazioni su pagina Master".


![È possibile configurare il titolo dalla finestra delle proprietà, troppo](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image2.png)

**Figura 02**: È possibile configurare il titolo dalla finestra delle proprietà, troppo


### <a name="setting-the-pages-title-programmatically"></a>Impostare il titolo della pagina a livello di codice

La pagina master `<head runat="server">` markup viene convertito in un [ `HtmlHead` classe](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlhead.aspx) istanza quando viene eseguito il rendering della pagina dal motore di ASP.NET. Il `HtmlHead` classe dispone di un [ `Title` proprietà](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlhead.title.aspx) il cui valore viene riflessa nell'oggetto sottoposto a rendering `<title>` elemento. Questa proprietà è accessibile dalla classe code-behind della pagina ASP.NET tramite `Page.Header.Title`; questo stessa proprietà è possibile accedere tramite `Page.Title`.

Per provare a impostare il titolo della pagina a livello di codice, passare al `About.aspx` codice della pagina classe e creare un gestore eventi per la pagina `Load` evento. Quindi, impostare il titolo della pagina su "esercitazioni pagina Master:: su:: *data*", dove *data* è la data corrente. Dopo aver aggiunto questo codice il `Page_Load` gestore dell'evento dovrebbe essere simile al seguente:


[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample4.cs)]

La figura 3 Mostra barra del titolo del browser quando si visita il `About.aspx` pagina.


![Il titolo della pagina è impostato a livello di codice e include la data corrente](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image3.png)

**Figura 03**: titolo della pagina di è impostato a livello di codice e include la data corrente


## <a name="step-2-automatically-assigning-a-page-title"></a>Passaggio 2: Assegnare automaticamente un titolo di pagina

Come illustrato nel passaggio 1, è possibile impostare il titolo della pagina in modo dichiarativo o a livello di codice. Se si dimentica di modificare in modo esplicito il titolo di un nome più descrittivo, tuttavia, la pagina avrà il titolo predefinito, "Senza nome pagina". In teoria, il titolo della pagina impostato automaticamente per noi nel caso in cui non è possibile specificare in modo esplicito il relativo valore. Se in fase di esecuzione il titolo della pagina è "Senza nome pagina", ad esempio, potrebbe essere opportuno avere il titolo viene aggiornato automaticamente per corrispondere al nome del file della pagina ASP.NET. Buone notizie sono quello con un minimo di lavoro iniziale, che è possibile che il titolo assegnato automaticamente.

Derivare tutte le pagine web ASP.NET di `Page` classe il `System.Web.UI` dello spazio dei nomi. Il `Page` classe definisce le funzionalità minime necessarie per una pagina ASP.NET ed espone le proprietà fondamentali come `IsPostBack`, `IsValid`, `Request`, e `Response`, tra molti altri. Spesso, ogni pagina in un'applicazione web richiede caratteristiche aggiuntive o funzionalità. Ciò fornisce un modo comune consiste nel creare una classe di base delle pagine personalizzate. Una classe personalizzata della pagina base è una classe create da cui deriva il `Page` classe e include funzionalità aggiuntive. Dopo aver creata questa classe di base, sono disponibili le pagine ASP.NET derivano da essa (anziché `Page` classe) in modo che offre le funzionalità estese per le pagine ASP.NET.

In questo passaggio è creare una pagina di base che imposta automaticamente il titolo della pagina al nome file della pagina ASP.NET, se il titolo non è in caso contrario stato impostato in modo esplicito. Passaggio 3 esamina l'impostazione del titolo della pagina in base alla mappa del sito.

> [!NOTE]
> Una conoscenza approfondita di creazione e utilizzo delle classi base della pagina personalizzata non rientra nell'ambito di questa serie di esercitazioni. Per altre informazioni, vedere [utilizzando una classe di Base personalizzata per le classi di Code-Behind di pagine ASP.NET](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx).


### <a name="creating-the-base-page-class"></a>Creazione della classe Base di pagina

La prima attività consiste nel creare una classe di base di pagina, che è una classe che estende la `Page` classe. Per iniziare, aggiungere un `App_Code` cartella al progetto facendo clic sul nome del progetto in Esplora soluzioni, scegliendo Aggiungi cartella ASP.NET, e quindi selezionando `App_Code`. Successivamente, fare clic su di `App_Code` cartella e aggiungere una nuova classe denominata `BasePage.cs`. La figura 4 Mostra Esplora soluzioni dopo il `App_Code` cartella e `BasePage.cs` classe sono stati aggiunti.


![Aggiungere una cartella App_Code e una classe denominata BasePage](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image4.png)

**Figura 04**: aggiungere un `App_Code` cartella e una classe denominata`BasePage`


> [!NOTE]
> Visual Studio supporta due modalità di gestione dei progetti: progetti di siti Web e progetti di applicazione Web. Il `App_Code` cartella è progettata per essere utilizzato con il modello di progetto di sito Web. Se si utilizza il modello di progetto di applicazione Web, inserire il `BasePage.cs` classe in una cartella con un nome diverso da `App_Code`, ad esempio `Classes`. Per ulteriori informazioni su questo argomento, fare riferimento a [la migrazione di un progetto di sito Web a un progetto di applicazione Web](http://webproject.scottgu.com/CSharp/Migration2/Migration2.aspx).


Poiché la pagina base personalizzata funge da classe base per classi code-behind ASP.NET pages, è necessario estendere la `Page` classe.


[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample5.cs)]

Ogni volta che viene richiesta una pagina ASP.NET procede attraverso una serie di fasi, che si conclude con la pagina richiesta, viene eseguito il rendering in HTML. È possibile toccare in una fase eseguendo l'override di `Page` della classe `OnEvent` metodo. Per la base di pagina verrà impostata automaticamente il titolo, se non è stato esplicitamente specificato dal `LoadComplete` fase (che, come è facile immaginare, si verifica dopo il `Load` fase).

A tale scopo, eseguire l'override di `OnLoadComplete` (metodo) e immettere il codice seguente:


[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample6.cs)]

Il `OnLoadComplete` metodo avvia determinando se il `Title` proprietà non è ancora stata impostata in modo esplicito. Se il `Title` proprietà `null`, una stringa vuota, o con il valore "Senza nome pagina", viene assegnato al nome del file della pagina ASP.NET richiesta. Il percorso fisico alla pagina richiesta ASP.NET - `C:\MySites\Tutorial03\Login.aspx`, ad esempio, è accessibile tramite la `Request.PhysicalPath` proprietà. Il `Path.GetFileNameWithoutExtension` metodo viene utilizzato per estrarre solo la parte del nome file e il nome del file viene quindi assegnato al `Page.Title` proprietà.

> [!NOTE]
> Invito di migliorare la logica per migliorare il formato del titolo. Ad esempio, se il nome del file della pagina è `Company-Products.aspx`, il codice sopra riportato produrrà il titolo "Prodotti della società", ma in teoria il trattino viene sostituito con uno spazio, come in "Prodotti della società". Inoltre, aggiungere uno spazio ogni volta che viene apportata una modifica di case. Vale a dire, provare ad aggiungere codice che trasforma il nome del file `OurBusinessHours.aspx` con un titolo di "l'orario lavorativo".


### <a name="having-the-content-pages-inherit-the-base-page-class"></a>Con le pagine di contenuto di ereditare la classe Base di pagina

È ora necessario aggiornare le pagine ASP.NET nel nostro sito da cui derivare la pagina base personalizzata (`BasePage`) anziché la `Page` classe. A tale scopo, passare a ogni classe code-behind e modificare la dichiarazione di classe da:


[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample7.cs)]

A:


[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample8.cs)]

Al termine dell'operazione, visitare il sito tramite un browser. Se si visita una pagina il cui titolo è impostato esplicitamente, ad esempio `Default.aspx` o `About.aspx`, viene usato il titolo specificato in modo esplicito. Se, tuttavia, si visita una pagina il cui titolo non è stato modificato da quello predefinito ("senza nome pagina"), la classe di base di pagina imposta il titolo al nome file della pagina.

La figura 5 mostra il `MultipleContentPlaceHolders.aspx` pagina quando viene visualizzato tramite un browser. Si noti che il titolo è precisamente nome file della pagina (meno l'estensione), "MultipleContentPlaceHolders".


[![Se non specificato in modo esplicito un titolo, nome del file della pagina viene utilizzato automaticamente](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image6.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image5.png)

**Figura 05**: se non specificato in modo esplicito un titolo, nome del file della pagina viene utilizzato automaticamente ([fare clic per visualizzare l'immagine ingrandita](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image7.png))


## <a name="step-3-basing-the-page-title-on-the-site-map"></a>Passaggio 3: Basare il titolo della pagina nella mappa del sito

ASP.NET fornisce un framework di mappa del sito affidabile che consente agli sviluppatori di pagina definire una mappa del sito gerarchica in una risorsa esterna (ad esempio una tabella di database o file XML) insieme ai controlli Web per la visualizzazione di informazioni relative alla mappa del sito (ad esempio, il controllo SiteMapPath, Menu e controlli TreeView).

La struttura della mappa del sito sono accessibili anche a livello di codice dalla classe code-behind della pagina ASP.NET. In questo modo è possibile impostare automaticamente il titolo della pagina per il titolo del nodo corrispondente nella mappa del sito. Consente di migliorare la `BasePage` classe creata nel passaggio 2 in modo che offre questa funzionalità. Ma è necessario innanzitutto creare una mappa del sito per il sito.

> [!NOTE]
> In questa esercitazione si presuppone che il lettore è già familiarità con ASP. Funzionalità della mappa del sito di rete. Per ulteriori informazioni sull'utilizzo della mappa del sito, consultare la serie di articoli multiparte, [esame ASP. Navigazione nel sito della rete](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx).


### <a name="creating-the-site-map"></a>Creazione della mappa del sito

Il sistema del sito viene compilato nella parte superiore di [modello provider](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), che separa la mappa del sito API dalla logica di serializzazione delle informazioni relative alla mappa del sito tra la memoria e un archivio permanente. .NET Framework viene fornito con il [ `XmlSiteMapProvider` classe](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx), ovvero il provider di mappa del sito predefinito. Come suggerisce il nome, `XmlSiteMapProvider` utilizza un file XML come archivio di mappa del sito. Di seguito è possibile utilizzare questo provider per definire la mappa del sito.

Iniziare creando un file di mappa del sito nella cartella radice del sito Web denominato `Web.sitemap`. A tale scopo, fare clic sul nome del sito Web in Esplora soluzioni, scegliere Aggiungi nuovo elemento e selezionare il modello di mappa del sito. Verificare che il file è denominato `Web.sitemap` e fare clic su Aggiungi.


[![Aggiungere un File sitemap alla cartella radice del sito Web](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image9.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image8.png)

**Figura 06**: aggiungere un File denominato `Web.sitemap` alla cartella radice del sito Web ([fare clic per visualizzare l'immagine ingrandita](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image10.png))


Aggiungere il seguente codice XML per il `Web.sitemap` file:


[!code-xml[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample9.xml)]

Questo codice XML definisce la struttura della mappa del sito gerarchica illustrata nella figura 7.


![Mappa del sito è attualmente composto di tre nodi](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image11.png)

**Figura 07**: la mappa del sito è attualmente composto di tre nodi


La struttura della mappa del sito in esercitazioni future verrà aggiornato man mano che si aggiungono nuovi esempi.

### <a name="updating-the-master-page-to-include-navigation-web-controls"></a>Aggiornamento della pagina Master per includere i controlli Web per la navigazione

Ora che è disponibile una mappa del sito definita, si aggiorna la pagina master per includere i controlli Web per la navigazione. In particolare, aggiungere un controllo ListView per la colonna a sinistra nella sezione lezioni che esegue il rendering di un elenco non ordinato con un elemento di elenco per ogni nodo definito nella mappa del sito.

> [!NOTE]
> Il controllo ListView è nuovo in ASP.NET versione 3.5. Se si utilizza una versione precedente di ASP.NET, è possibile utilizzare il controllo Repeater. Per ulteriori informazioni sul controllo ListView, vedere [Using ASP.NET 3.5 controlli ListView e DataPager](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx).


Avviare rimuovendo il markup di elenco non ordinato esistente dalla sezione lezioni. Successivamente, trascinare un controllo ListView dalla casella degli strumenti e rilasciarlo sotto le lezioni sull'intestazione. ListView si trova nella sezione dati della casella degli strumenti, insieme ad altri controlli di visualizzazione: GridView, DetailsView e FormView. Impostare la proprietà ID del controllo ListView `LessonsList`.

Scegliere la configurazione guidata origine dati per associare il controllo ListView a un nuovo controllo SiteMapDataSource denominato `LessonsDataSource`. Il controllo SiteMapDataSource restituisce la struttura gerarchica dal sistema di mappa del sito.


[![Associare un controllo SiteMapDataSource al controllo LessonsList ListView](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image13.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image12.png)

**Figura 08**: associare un controllo SiteMapDataSource il `LessonsList` controllo ListView ([fare clic per visualizzare l'immagine ingrandita](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image14.png))


Dopo aver creato il controllo SiteMapDataSource, è necessario definire modelli di ListView, in modo che esegue il rendering di un elenco non ordinato con un elemento di elenco per ogni nodo restituito dal controllo SiteMapDataSource. Questa può essere eseguita tramite il seguente markup del modello:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample10.aspx)]

Il `LayoutTemplate` genera il markup per un elenco non ordinato (`<ul>...</ul>`) mentre il `ItemTemplate` esegue il rendering di ogni elemento restituito da SiteMapDataSource come voce di elenco (`<li>`) che contiene un collegamento alla lezione particolare.

Dopo aver configurato i modelli di ListView, visitare il sito Web. Come illustrato nella figura 9, la sezione lezioni contiene un singolo elemento puntato, Home. Dove si trovano le informazioni e l'utilizzo di lezioni ContentPlaceHolder più controlli? SiteMapDataSource è progettato per restituire un set gerarchico di dati, ma il controllo ListView può visualizzare solo un singolo livello della gerarchia. Di conseguenza, viene visualizzato solo il primo livello dei nodi della mappa del sito restituiti da SiteMapDataSource.


[![La sezione lezioni contiene un singolo elemento dell'elenco](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image16.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image15.png)

**Figura 09**: la sezione lezioni contiene un singolo elemento di elenco ([fare clic per visualizzare l'immagine ingrandita](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image17.png))


Per visualizzare più livelli è possibile nidificare più controlli ListView all'interno di `ItemTemplate`. Questa tecnica è stata esaminata nel [ *pagine Master e esplorazione del sito* esercitazione](../../data-access/introduction/master-pages-and-site-navigation-cs.md) di my [utilizzo di una serie di esercitazioni dati](../../data-access/index.md). Tuttavia, per questa serie di esercitazioni la mappa del sito contiene solo un due livelli: Home (il livello principale); e ogni lezione come elemento figlio della home page. Anziché la creazione di un ListView annidati, è invece possibile indicare a non restituire il nodo di inizio impostando SiteMapDataSource relativo [ `ShowStartingNode` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemapdatasource.showstartingnode.aspx) a `false`. L'effetto finale è che il controllo SiteMapDataSource inizia restituendo di secondo livello di nodi della mappa del sito.

Con questa modifica, ListView consente di visualizzare i punti elenco per informazioni e utilizzo di più controlli ContentPlaceHolder lezioni, ma omette un punto elenco per la home page. Per risolvere questo problema, è possibile aggiungere un punto elenco in modo esplicito per la home page nel `LayoutTemplate`:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample11.aspx)]

Configurando SiteMapDataSource per omettere il nodo di inizio e l'aggiunta esplicita di un punto iniziale, nella sezione di lezioni viene ora visualizzato l'output desiderato.


[![La sezione lezioni contiene un punto elenco per la casa e ogni nodo figlio](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image19.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image18.png)

**Figura 10**: la sezione lezioni contiene un punto elenco per la casa e ciascun nodo figlio ([fare clic per visualizzare l'immagine ingrandita](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image20.png))


### <a name="setting-the-title-based-on-the-site-map"></a>Impostazione del titolo in base alla mappa del sito

Con la mappa del sito sul posto, è possibile aggiornare il nostro `BasePage` classe da utilizzare il titolo specificato nella mappa del sito. Come è stato fatto nel passaggio 2, si desidera utilizzare solo il titolo del nodo della mappa del sito se il titolo della pagina non è stato impostato in modo esplicito dallo sviluppatore della pagina. Se la pagina richiesta non ha impostato in modo esplicito titolo di pagina e non viene trovato nella mappa del sito, quindi si tornerà alla con un nome della pagina richiesta (meno l'estensione), come è stato fatto nel passaggio 2. Questo processo di decisione illustrato nella figura 11.


![In assenza di un in modo esplicito impostare titolo della pagina, viene utilizzato il titolo del corrispondente nodo della mappa del sito](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image21.png)

**Figura 11**: In assenza di un in modo esplicito impostare titolo della pagina, viene utilizzato il titolo del corrispondente nodo della mappa del sito


Aggiornamento di `BasePage` della classe `OnLoadComplete` metodo per includere il codice seguente:


[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample12.cs)]

Come in precedenza, il `OnLoadComplete` metodo avvia determinando se il titolo della pagina è stato impostato in modo esplicito. Se `Page.Title` è `null`, una stringa vuota, viene assegnato il valore "Senza nome pagina", quindi il codice assegna automaticamente un valore a o `Page.Title`.

Per determinare il titolo da utilizzare, il codice di avvio facendo riferimento al [ `SiteMap` classe](https://msdn.microsoft.com/library/system.web.sitemap.aspx)del [ `CurrentNode` proprietà](https://msdn.microsoft.com/library/system.web.sitemap.currentnode.aspx). `CurrentNode`Restituisce il [ `SiteMapNode` ](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx) istanza nella mappa del sito che corrisponde alla pagina attualmente richiesta. Presupponendo che la pagina attualmente richiesta si trova all'interno della mappa del sito, il `SiteMapNode`del `Title` proprietà viene assegnata al titolo della pagina. Se la pagina attualmente richiesta non è presente nella mappa del sito, `CurrentNode` restituisce `null` e nome file della pagina richiesta viene utilizzato come il titolo (come è stato completato nel passaggio 2).

Figura 12 illustra il `MultipleContentPlaceHolders.aspx` pagina quando viene visualizzato tramite un browser. Poiché il titolo della pagina non viene impostato in modo esplicito, viene invece utilizzato il titolo del nodo della mappa del sito corrispondente.


![Titolo della pagina MultipleContentPlaceHolders.aspx viene effettuato il pull della mappa](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image22.png)

**Figura 12**: il `MultipleContentPlaceHolders.aspx` titolo della pagina viene effettuato il pull della mappa


## <a name="step-4-adding-other-page-specific-markup-to-theheadsection"></a>Passaggio 4: Aggiunta di altri Markup specifico della pagina per il`<head>`sezione

I passaggi 1, 2 e 3 esaminato personalizzazione di `<title>` elemento in base a una pagina per pagina. Oltre a `<title>`, `<head>` sezione può contenere `<meta>` elementi e `<link>` elementi. Come accennato in precedenza in questa esercitazione, `Site.master`del `<head>` sezione include un `<link>` elemento `Styles.css`. Poiché questo `<link>` è definito l'elemento all'interno della pagina master, è incluso nel `<head>` sezione per tutte le pagine di contenuto. Ma come passiamo sull'aggiunta di `<meta>` e `<link>` gli elementi in base a una pagina per pagina?

Il modo più semplice per aggiungere contenuto specifico della pagina per il `<head>` sezione, è possibile creare un controllo ContentPlaceHolder nella pagina master. Abbiamo già tali un ContentPlaceHolder (denominato `head`). Pertanto, per aggiungere personalizzato `<head>` markup, creare un corrispondente controllo nella pagina contenuto e posizionarvi il markup.

Per illustrare personalizzata aggiunta `<head>` markup a una pagina, si includono un `<meta>` elemento description con il set corrente di pagine di contenuto. Il `<meta>` elemento description fornisce una breve descrizione della pagina web, la maggior parte dei motori di ricerca incorporano tali informazioni in diversi formati, quando si visualizzano i risultati della ricerca.

Oggetto `<meta>` elemento description ha il formato seguente:


[!code-html[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample13.html)]

Per aggiungere il markup per una pagina di contenuto, aggiungere il testo precedente del controllo contenuto che viene eseguito il mapping intestazione della pagina master ContentPlaceHolder. Ad esempio, per definire un `<meta>` elemento description per `Default.aspx`, aggiungere il markup seguente:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample14.aspx)]

Poiché l'intestazione ContentPlaceHolder non all'interno del corpo della pagina HTML, il codice aggiunto al controllo del contenuto non viene visualizzato nella visualizzazione progettazione. Per visualizzare il `<meta>` descrizione elemento visita `Default.aspx` tramite un browser. Dopo il caricamento della pagina, visualizzare il codice sorgente e si noti che il `<head>` sezione include il markup specificato nel controllo contenuto.

È opportuno aggiungere `<meta>` elementi descrizione `About.aspx`, `MultipleContentPlaceHolders.aspx`, e `Login.aspx`.

### <a name="programmatically-adding-markup-to-theheadregion"></a>Aggiunta a livello di programmazione di Markup per il`<head>`area

Head ContentPlaceHolder consente di aggiungere in modo dichiarativo di markup personalizzata della pagina master `<head>` area. Markup personalizzata può anche essere aggiunta a livello di codice. Tenere presente che il `Page` della classe `Header` proprietà restituisce il `HtmlHead` istanza definita nella pagina master (il `<head runat="server">`).

In grado di aggiungere contenuto a livello di codice il `<head>` area è utile quando il contenuto da aggiungere è dinamico. Ad esempio si basa sull'utente visita la pagina. forse vengono recuperato da un database. A prescindere dal motivo, è possibile aggiungere contenuto per il `HtmlHead` con l'aggiunta di controlli per la raccolta di controlli come illustrato di seguito:


[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample15.cs)]

Il codice sopra riportato aggiunge il `<meta>` elemento parole chiave per la `<head>` area, che fornisce un elenco delimitato da virgole delle parole chiave che descrivono la pagina. Si noti che per aggiungere un `<meta>` tag crei un [ `HtmlMeta` ](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlmeta.aspx) dell'istanza, impostare il relativo `Name` e `Content` proprietà e quindi aggiungerlo al `Header`del `Controls` insieme. Analogamente, a livello di codice aggiungere un `<link>` elemento, creare un [ `HtmlLink` ](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmllink.aspx) dell'oggetto, impostarne le proprietà e quindi aggiungerlo al `Header`del `Controls` insieme.

> [!NOTE]
> Per aggiungere un codice arbitrario, creare un [ `LiteralControl` ](https://msdn.microsoft.com/library/system.web.ui.literalcontrol.aspx) dell'istanza, impostare il relativo `Text` proprietà e quindi aggiungerlo al `Header`del `Controls` insieme.


## <a name="summary"></a>Riepilogo

In questa esercitazione illustra diversi modi per aggiungere `<head>` markup area in base a una pagina per pagina. Una pagina master deve includere un `HtmlHead` istanza (`<head runat="server">`) con un ContentPlaceHolder. Il `HtmlHead` istanza consente le pagine di contenuto per l'accesso a livello di codice il `<head>` area e impostare la pagina in modo dichiarativo e a livello di codice il titolo del; il controllo ContentPlaceHolder consente di markup personalizzata da aggiungere al `<head>` sezione in modo dichiarativo tramite un controllo contenuto.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Impostazione in modo dinamico il titolo della pagina in ASP.NET](http://aspnet.4guysfromrolla.com/articles/051006-1.aspx)
- [Esame di ASP. Esplorazione del sito di rete](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Come utilizzare il tag HTML Meta](http://searchenginewatch.com/showPage.html?page=2167931)
- [Pagine master in ASP.NET](http://www.odetocode.com/articles/419.aspx)
- [Utilizzo di ASP.NET 3.5 controlli ListView e DataPager](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)
- [Utilizzo di una classe di Base personalizzata per le classi di Code-Behind di pagine di ASP.NET](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx)

### <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di più libri e fondatore di 4GuysFromRolla, ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 3.5 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott può essere raggiunto al [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) o tramite il suo blog all'indirizzo [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Lead revisori per questa esercitazione sono stati Zack Jones e Suchi Banerjee. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

>[!div class="step-by-step"]
[Precedente](multiple-contentplaceholders-and-default-content-cs.md)
[Successivo](urls-in-master-pages-cs.md)
