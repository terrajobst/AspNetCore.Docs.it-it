---
uid: web-forms/overview/data-access/database-driven-site-maps/building-a-custom-database-driven-site-map-provider-cs
title: Creazione di un Provider di mappa del sito personalizzato basato su Database (c#) | Microsoft Docs
author: rick-anderson
description: Il provider di mappa del sito predefinito in ASP.NET 2.0 consente di recuperare i dati da un file XML statico. Mentre il provider basato su XML è adatto a molte piccole e medie-finestra...
ms.author: aspnetcontent
ms.date: 06/26/2007
ms.assetid: 04b7591d-106f-4f05-87e9-d416cb65a8a6
msc.legacyurl: /web-forms/overview/data-access/database-driven-site-maps/building-a-custom-database-driven-site-map-provider-cs
msc.type: authoredcontent
ms.openlocfilehash: fdc6ddeac800279a638911f1f64772c49fff5449
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807112"
---
<a name="building-a-custom-database-driven-site-map-provider-c"></a>Creazione di un Provider di mappa del sito personalizzato basato su Database (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare il codice](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_62_CS.zip) o [Scarica il PDF](building-a-custom-database-driven-site-map-provider-cs/_static/datatutorial62cs1.pdf)

> Il provider di mappa del sito predefinito in ASP.NET 2.0 consente di recuperare i dati da un file XML statico. Mentre il provider basato su XML è adatto a molti siti Web di piccoli e medie, grandi applicazioni Web richiedono una maggiore dinamicità mappa del sito. In questa esercitazione che creeremo un provider della mappa del sito personalizzato che recupera i dati dal livello della logica di Business, che a sua volta recupera i dati dal database.


## <a name="introduction"></a>Introduzione

ASP.NET 2.0 consente a funzionalità di mappa del sito s uno sviluppatore di pagina definire una mappa del sito web dell'applicazione s in un supporto permanente, ad esempio in un file XML. Una volta definito, i dati della mappa del sito sono accessibili a livello di programmazione tramite le [ `SiteMap` classe](https://msdn.microsoft.com/library/system.web.sitemap.aspx) nel [ `System.Web` dello spazio dei nomi](https://msdn.microsoft.com/library/system.web.aspx) o tramite un'ampia gamma di navigazione Web controlli, ad esempio il Controlli SiteMapPath, Menu e TreeView. Il sistema del sito Usa il [modello di provider](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx) in modo che le implementazioni di serializzazione mappa del sito diversi possono essere create e collegate in un'applicazione web. Il provider di mappa del sito predefinito fornito con ASP.NET 2.0 rende persistenti struttura della mappa del sito in un file XML. Nel [pagine Master e spostamento nel sito](../introduction/master-pages-and-site-navigation-cs.md) esercitazione è stato creato un file denominato `Web.sitemap` che contiene questa struttura e hanno deciso di aggiornare il codice XML con ogni nuova sezione dell'esercitazione.

Il provider di mappa del sito basati su XML predefinito funziona bene se struttura s della mappa del sito è abbastanza statica, ad esempio per queste esercitazioni. In molti scenari, tuttavia, è necessaria una maggiore dinamicità mappa del sito. Prendere in considerazione la mappa del sito illustrata nella figura 1, dove ogni categoria e prodotto vengono visualizzati come sezioni nella struttura s sito Web. Con questa mappa del sito, visitare la pagina web corrispondente al nodo radice potrebbe elencare tutte le categorie, considerando che visitano una pagina web particolare categoria s verrà visualizzato l'elenco di prodotti s categoria e la visualizzazione di una pagina web di quel particolare prodotto s mostrerebbe prodotto s-dettagli.


[![Le categorie e composizione prodotti la struttura della mappa s sito](building-a-custom-database-driven-site-map-provider-cs/_static/image1.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image1.png)

**Figura 1**: The categorie e i prodotti struttura la mappa del sito s struttura ([fare clic per visualizzare l'immagine con dimensioni normali](building-a-custom-database-driven-site-map-provider-cs/_static/image2.png))


Anche se questa struttura e prodotto dal basato sulle categorie può essere impostate come hardcoded nel `Web.sitemap` file, il file dovrà essere aggiornata ogni volta che una categoria o prodotto è stato aggiunto, rimosso o rinominato. Di conseguenza, la manutenzione delle mappe del sito potrebbe essere notevolmente semplificata se la struttura è stata recuperata dal database o, in teoria, dal livello della logica di Business dell'architettura s dell'applicazione. In questo modo, man mano che sono stati aggiunti i prodotti e le categorie, rinominato o eliminato, la mappa del sito consente l'aggiornamento automatico per riflettere queste modifiche.

Poiché la serializzazione della mappa del sito di ASP.NET 2.0 s viene compilata sopra il modello di provider, è possibile creare un provider della mappa del sito personalizzato che acquisisce i dati da un archivio dati alternativo, ad esempio il database o l'architettura. In questa esercitazione si creerà un provider personalizzato che recupera i relativi dati dalla classe BLL. Introduzione a ti permettono di s.

> [!NOTE]
> Il provider della mappa del sito personalizzato creato in questa esercitazione è strettamente accoppiato al modello di architettura e i dati s dell'applicazione. Jeff Prosise 1!s [l'archiviazione delle mappe dei siti in SQL Server](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) e [SQL Site Map Provider è già stato di attesa per](https://msdn.microsoft.com/msdnmag/issues/06/02/wickedcode/default.aspx) articoli esaminare un approccio generalizzato per l'archiviazione dei dati della mappa del sito in SQL Server.


## <a name="step-1-creating-the-custom-site-map-provider-web-pages"></a>Passaggio 1: Creazione di pagine Web Provider della mappa del sito personalizzata

Prima di iniziare la creazione di un provider della mappa del sito personalizzato, consentire s prima di tutto aggiungere le pagine ASP.NET, che sarà necessario per questa esercitazione. Iniziare aggiungendo una nuova cartella denominata `SiteMapProvider`. Successivamente, aggiungere le seguenti pagine ASP.NET per quella cartella, assicurandosi di associare ogni pagina con il `Site.master` pagina master:

- `Default.aspx`
- `ProductsByCategory.aspx`
- `ProductDetails.aspx`

Aggiungere anche un `CustomProviders` sottocartella di `App_Code` cartella.


![Aggiungere le pagine ASP.NET per le esercitazioni di correlate al Provider della mappa del sito](building-a-custom-database-driven-site-map-provider-cs/_static/image2.gif)

**Figura 2**: aggiungere le pagine ASP.NET per il sito eseguire il mapping di esercitazioni correlate al Provider


Perché è presente un solo esercitazione di questa sezione, non abbiamo bisogno di t `Default.aspx` per elencare le esercitazioni di sezione s. Al contrario, `Default.aspx` visualizzerà le categorie in un controllo GridView. Che verranno affrontati in questo passaggio 2.

A questo punto, aggiornare `Web.sitemap` per includere un riferimento di `Default.aspx` pagina. In particolare, aggiungere il markup seguente dopo la memorizzazione nella cache `<siteMapNode>`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample1.xml)]

Dopo aver aggiornato `Web.sitemap`, si consiglia di visualizzare il sito Web di esercitazioni tramite un browser. Il menu a sinistra ora include un elemento per l'esercitazione di provider della mappa sito esclusiva.


![Mappa del sito include ora una voce per l'esercitazione di Provider della mappa del sito](building-a-custom-database-driven-site-map-provider-cs/_static/image3.gif)

**Figura 3**: la mappa del sito include ora una voce per l'esercitazione di Provider della mappa del sito


Questo aspetto principale dell'esercitazione s è per illustrare la creazione di un provider di mappa del sito personalizzata e configurazione di un'applicazione web per usare tale provider. In particolare, creeremo un provider che restituisce una mappa del sito che include un nodo radice insieme a un nodo per ogni categoria e il prodotto, come illustrato nella figura 1. In generale, ogni nodo nella mappa del sito può specificare un URL. Per la mappa del sito, sarà l'URL radice del nodo s `~/SiteMapProvider/Default.aspx`, che elenca tutte le categorie nel database. Ogni nodo della categoria nella mappa del sito avrà un URL che punta a `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID=categoryID`, che elenca tutti i prodotti nell'oggetto specificato *categoryID*. Infine, ogni nodo della mappa del sito di prodotto punterà a `~/SiteMapProvider/ProductDetails.aspx?ProductID=productID`, che consentirà di visualizzare i dettagli del prodotto specifico s.

Per iniziare, dobbiamo creare la `Default.aspx`, `ProductsByCategory.aspx`, e `ProductDetails.aspx` pagine. Queste pagine vengono eseguite nei passaggi 2, 3 e 4, rispettivamente. Poiché tramite questa esercitazione è nel provider di mappa del sito, e poiché esercitazioni precedenti hanno illustrato la creazione di questi tipi di Multi-pagina master-details report, si verrà Affrettati tramite i passaggi 2 e 4. Se è necessario un aggiornamento sulla creazione di report master o di dettaglio che si estendono su più pagine, vedere la [Master/Dettagli filtro in due pagine](../masterdetail/master-detail-filtering-across-two-pages-cs.md) esercitazione.

## <a name="step-2-displaying-a-list-of-categories"></a>Passaggio 2: Visualizzare un elenco di categorie

Aprire il `Default.aspx` nella pagina la `SiteMapProvider` cartelle e trascinare un controllo GridView dalla casella degli strumenti nella finestra di progettazione, l'impostazione relativa `ID` a `Categories`. GridView s nello smart tag, associarlo a un nuovo oggetto ObjectDataSource denominato `CategoriesDataSource` e configurarlo in modo che recuperi relativi dati tramite il `CategoriesBLL` classe s `GetCategories` (metodo). Poiché questo controllo GridView solo Visualizza le categorie e non fornisce funzionalità di modifica dei dati, impostare gli elenchi a discesa nell'aggiornamento, inserimento ed eliminare schede su (nessuno).


[![Configurare ObjectDataSource per restituire le categorie usando il metodo GetCategories](building-a-custom-database-driven-site-map-provider-cs/_static/image4.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image3.png)

**Figura 4**: configurare ObjectDataSource per restituire le categorie usando il `GetCategories` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](building-a-custom-database-driven-site-map-provider-cs/_static/image4.png))


[![Impostare gli elenchi a discesa nell'aggiornamento, inserimento ed eliminare schede su (nessuno)](building-a-custom-database-driven-site-map-provider-cs/_static/image5.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image5.png)

**Figura 5**: impostare l'elenco a discesa sono elencati nell'aggiornamento, inserimento ed eliminare schede su (nessuno) ([fare clic per visualizzare l'immagine con dimensioni normali](building-a-custom-database-driven-site-map-provider-cs/_static/image6.png))


Dopo aver completato la procedura guidata Configura origine dati, Visual Studio aggiungerà un BoundField per `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`, e `BrochurePath`. Modificare il controllo GridView in modo che contenga solo le `CategoryName` e `Description` BoundField e aggiornare le `CategoryName` BoundField s `HeaderText` proprietà per categoria.

Successivamente, aggiungere un HyperLinkField e posizionarlo in modo che venga s il campo più a sinistra. Impostare il `DataNavigateUrlFields` proprietà `CategoryID` e il `DataNavigateUrlFormatString` proprietà `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID={0}`. Impostare il `Text` proprietà per visualizzare i prodotti.


![Aggiungere un HyperLinkField a GridView categorie](building-a-custom-database-driven-site-map-provider-cs/_static/image6.gif)

**Figura 6**: aggiungere un HyperLinkField al `Categories` GridView


Dopo la creazione di ObjectDataSource e personalizzare i campi s GridView, il markup dichiarativo di due controlli avrà un aspetto simile al seguente:


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample2.aspx)]

Figura 7 mostra `Default.aspx` quando viene visualizzato tramite un browser. Facendo clic su una categoria s vista prodotti collegamento reindirizza alla `ProductsByCategory.aspx?CategoryID=categoryID`, che verrà creata nel passaggio 3.


[![Ogni categoria viene elencata insieme con un collegamento di prodotti di visualizzazione](building-a-custom-database-driven-site-map-provider-cs/_static/image7.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image7.png)

**Figura 7**: ogni categoria viene elencata insieme con un collegamento di prodotti di visualizzazione ([fare clic per visualizzare l'immagine con dimensioni normali](building-a-custom-database-driven-site-map-provider-cs/_static/image8.png))


## <a name="step-3-listing-the-selected-category-s-products"></a>Passaggio 3: Elenco dei prodotti di s categoria selezionata

Aprire il `ProductsByCategory.aspx` pagina e aggiungere un controllo GridView, denominarlo `ProductsByCategory`. Dal suo smart tag, associare il controllo GridView per un nuovo oggetto ObjectDataSource denominato `ProductsByCategoryDataSource`. Configurare ObjectDataSource per usare la `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` (metodo) e impostare l'elenco a discesa Elenca su (nessuno) nelle schede UPDATE, INSERT e DELETE.


[![Usare il metodo di classe ProductsBLL s GetProductsByCategoryID(categoryID)](building-a-custom-database-driven-site-map-provider-cs/_static/image8.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image9.png)

**Figura 8**: usare il `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](building-a-custom-database-driven-site-map-provider-cs/_static/image10.png))


Il passaggio finale della procedura guidata Configura origine dati richiede un'origine di parametro per *categoryID*. Poiché queste informazioni vengono passate tramite il campo querystring `CategoryID`, selezionare QueryString dall'elenco a discesa e immettere CategoryID nella casella di testo QueryStringField, come illustrato nella figura 9. Fare clic su Fine per completare la procedura guidata.


[![Usare il campo Querystring CategoryID per il parametro categoryID](building-a-custom-database-driven-site-map-provider-cs/_static/image9.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image11.png)

**Figura 9**: usare il `CategoryID` Querystring Field per il *categoryID* parametro ([fare clic per visualizzare l'immagine con dimensioni normali](building-a-custom-database-driven-site-map-provider-cs/_static/image12.png))


Dopo aver completato la procedura guidata, Visual Studio aggiungerà BoundField corrispondente e un CampoCasellaDiControllo a GridView per i campi di dati del prodotto. Rimuovi tutto tranne le `ProductName`, `UnitPrice`, e `SupplierName` BoundField. Personalizzare questi tre BoundField `HeaderText` proprietà leggere fornitore, prodotto e prezzo. Formato di `UnitPrice` BoundField come valuta.

Successivamente, aggiungere un HyperLinkField e spostarlo nella posizione più a sinistra. Impostare relativi `Text` proprietà per visualizzare i dettagli, relativo `DataNavigateUrlFields` proprietà `ProductID`e la relativa `DataNavigateUrlFormatString` proprietà `~/SiteMapProvider/ProductDetails.aspx?ProductID={0}`.


![Aggiungere un HyperLinkField i dettagli di visualizzazione che punta a ProductDetails.aspx](building-a-custom-database-driven-site-map-provider-cs/_static/image10.gif)

**Figura 10**: aggiungere una visualizzazione dettagli HyperLinkField che punta a `ProductDetails.aspx`


Dopo aver apportato queste personalizzazioni, GridView e ObjectDataSource s markup dichiarativo sarà simile al seguente:


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample3.aspx)]

Tornare alla visualizzazione `Default.aspx` tramite un browser e fare clic su Visualizza prodotti link per Beverages. Si passerà a `ProductsByCategory.aspx?CategoryID=1`, visualizzazione di nomi, i prezzi e fornitori dei prodotti nel database Northwind che appartengono alla categoria Beverages (vedere la figura 11). È possibile migliorare ulteriormente questa pagina per includere un collegamento per restituire gli utenti alla pagina dell'inserzione categoria (`Default.aspx`) e un controllo DetailsView o FormView che visualizza il nome della categoria selezionata s e una descrizione.


[![Vengono visualizzati i nomi delle bibite, prezzi e fornitori](building-a-custom-database-driven-site-map-provider-cs/_static/image11.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image13.png)

**Figura 11**: vengono visualizzati i nomi delle bibite The, i prezzi e fornitori ([fare clic per visualizzare l'immagine con dimensioni normali](building-a-custom-database-driven-site-map-provider-cs/_static/image14.png))


## <a name="step-4-showing-a-product-s-details"></a>Passaggio 4: Visualizzazione di un prodotto s-dettagli

La pagina finale, `ProductDetails.aspx`, vengono visualizzati i dettagli di prodotti selezionati. Apri `ProductDetails.aspx` e trascinare un controllo DetailsView dalla casella degli strumenti nella finestra di progettazione. Impostare la s DetailsView `ID` proprietà `ProductInfo` e cancellare relativo `Height` e `Width` i valori delle proprietà. Dal suo smart tag, associare un nuovo oggetto ObjectDataSource denominato DetailsView `ProductDataSource`, la configurazione di ObjectDataSource per estrarre i dati dal `ProductsBLL` classe s `GetProductByProductID(productID)` (metodo). Come con le pagine web precedente creato nei passaggi 2 e 3, impostare gli elenchi a discesa nell'aggiornamento, inserimento ed eliminare schede su (nessuno).


[![Configurare ObjectDataSource per usare il metodo GetProductByProductID(productID)](building-a-custom-database-driven-site-map-provider-cs/_static/image12.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image15.png)

**Figura 12**: configurare ObjectDataSource per usare il `GetProductByProductID(productID)` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](building-a-custom-database-driven-site-map-provider-cs/_static/image16.png))


L'ultimo passaggio della procedura guidata Configura origine dati viene richiesto per l'origine del *productID* parametro. Poiché questi dati provengono da campo querystring `ProductID`, impostare l'elenco a discesa per la casella di testo QueryStringField per ProductID e stringa di query. Infine, fare clic sul pulsante Fine per completare la procedura guidata.


[![Configurare i parametri per estrarre il valore del campo Querystring ProductID productID](building-a-custom-database-driven-site-map-provider-cs/_static/image13.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image17.png)

**Figura 13**: configurare il *productID* parametro per eseguire il Pull il relativo valore dal `ProductID` Querystring Field ([fare clic per visualizzare l'immagine con dimensioni normali](building-a-custom-database-driven-site-map-provider-cs/_static/image18.png))


Dopo aver completato la procedura guidata Configura origine dati, Visual Studio creerà BoundField corrispondente e un CampoCasellaDiControllo in DetailsView per i campi di dati del prodotto. Rimuovere il `ProductID`, `SupplierID`, e `CategoryID` BoundField e configurare i campi rimanenti come desiderato. Dopo un numero limitato di configurazioni estetici, il markup dichiarativo s DetailsView e ObjectDataSource era simile al seguente:


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample4.aspx)]

Per testare questa pagina, tornare a `Default.aspx` e fare clic sulla visualizzazione dei prodotti per la categoria Beverages. Nell'elenco dei prodotti di bevande, fare clic sul collegamento Visualizza dettagli per Chai tè. Si passerà a `ProductDetails.aspx?ProductID=1`, che mostra un s Chai tè dettagli (vedere la figura 14).


[![Viene visualizzato il tè Chai s fornitore, categoria, prezzi e altre informazioni](building-a-custom-database-driven-site-map-provider-cs/_static/image14.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image19.png)

**Figura 14**: viene visualizzato il tè Chai s fornitore, categoria, prezzi e altre informazioni ([fare clic per visualizzare l'immagine con dimensioni normali](building-a-custom-database-driven-site-map-provider-cs/_static/image20.png))


## <a name="step-5-understanding-the-inner-workings-of-a-site-map-provider"></a>Passaggio 5: Comprendere i meccanismi interni di un Provider di mappa del sito

Mappa del sito è rappresentata nella memoria del server s web come una raccolta di `SiteMapNode` istanze che formano una gerarchia. Deve essere presente esattamente una radice, tutti i nodi non radice devono avere esattamente un nodo padre e tutti i nodi possono avere un numero arbitrario di elementi figlio. Ogni `SiteMapNode` oggetto rappresenta una sezione nella struttura del sito Web s; queste sezioni hanno in genere una pagina web corrispondente. Di conseguenza, il [ `SiteMapNode` classe](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx) dispone di proprietà quali `Title`, `Url`, e `Description`, che forniscono informazioni per la sezione di `SiteMapNode` rappresenta. È inoltre disponibile un' `Key` proprietà che identifica in modo univoco ogni `SiteMapNode` nella gerarchia, nonché proprietà usate per definire questa gerarchia `ChildNodes`, `ParentNode`, `NextSibling`, `PreviousSibling`e così via.

Figura 15 viene illustrata la struttura di mappa sito generale dalla figura 1, ma con i dettagli di implementazione descritto in dettaglio più preciso.


[![Ogni SiteMapNode dispone di proprietà, ad esempio titolo, Url, chiave e così via](building-a-custom-database-driven-site-map-provider-cs/_static/image16.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image15.gif)

**Figura 15**: ogni `SiteMapNode` ha le proprietà, ad esempio `Title`, `Url`, `Key`e così via ([fare clic per visualizzare l'immagine con dimensioni normali](building-a-custom-database-driven-site-map-provider-cs/_static/image17.gif))


Mappa del sito è accessibile tramite il [ `SiteMap` classe](https://msdn.microsoft.com/library/system.web.sitemap.aspx) nel [ `System.Web` dello spazio dei nomi](https://msdn.microsoft.com/library/system.web.aspx). Questa classe s `RootNode` viene restituita la radice della mappa s sito `SiteMapNode` esempio. `CurrentNode` restituisce il `SiteMapNode` cui `Url` proprietà corrisponda all'URL della pagina attualmente richiesta. Questa classe viene utilizzata internamente dai controlli Web di ASP.NET 2.0 s navigazione.

Quando il `SiteMap` per accedere a proprietà di classe s, è necessario serializzare la struttura della mappa del sito da un supporto permanente in memoria. Tuttavia, la logica di serializzazione della mappa del sito non è hardcoded nel `SiteMap` classe. In alternativa, in fase di esecuzione la `SiteMap` classe determina quale mappa del sito *provider* da utilizzare per la serializzazione. Per impostazione predefinita, il [ `XmlSiteMapProvider` classe](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx) viene usato, che legge la struttura della mappa s sito da un file XML formattato correttamente. Tuttavia, con un po' di lavoro è possibile creare un provider della mappa del sito personalizzato.

Tutti i provider di mappa del sito deve essere derivato dal [ `SiteMapProvider` classe](https://msdn.microsoft.com/library/system.web.sitemapprovider.aspx), che include i metodi essenziali e le proprietà necessarie per il sito provider della mappa, ma omette molti dei dettagli di implementazione. Una seconda classe, [ `StaticSiteMapProvider` ](https://msdn.microsoft.com/library/system.web.staticsitemapprovider.aspx), estende il `SiteMapProvider` classe e contiene un'implementazione più solida funzionalità necessari. Internamente, il `StaticSiteMapProvider` archivia il `SiteMapNode` mapping delle istanze del sito un `Hashtable` e fornisce metodi quali `AddNode(child, parent)`, `RemoveNode(siteMapNode),` e `Clear()` che aggiungono e rimuovono `SiteMapNode` s alla classe interna `Hashtable`. L'oggetto `XmlSiteMapProvider` è derivato da `StaticSiteMapProvider`.

Quando la creazione di un provider della mappa del sito personalizzato che estende `StaticSiteMapProvider`, sono disponibili due metodi astratti che devono essere sottoposto a override: [ `BuildSiteMap` ](https://msdn.microsoft.com/library/system.web.staticsitemapprovider.buildsitemap.aspx) e [ `GetRootNodeCore` ](https://msdn.microsoft.com/library/system.web.sitemapprovider.getrootnodecore.aspx). `BuildSiteMap`, come suggerisce il nome, è responsabile per il caricamento di struttura della mappa del sito da un archivio permanente e costruzione, in memoria. `GetRootNodeCore` Restituisce il nodo radice della mappa del sito.

Prima di un sito web l'applicazione può usare un provider di mappa del sito che deve essere registrato nella configurazione di s dell'applicazione. Per impostazione predefinita, il `XmlSiteMapProvider` classe registrata con il nome `AspNetXmlSiteMapProvider`. Per registrare i provider della mappa del sito aggiuntivi, aggiungere il markup seguente alla `Web.config`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample5.xml)]

Il *name* valore viene assegnato un nome leggibile dall'utente al provider durante *tipo* specifica il nome del tipo completo di provider della mappa del sito. Esploreremo valori concreti per la *name* e *tipo* valori nel passaggio 7, dopo abbiamo creato il provider della mappa del sito personalizzato.

La classe del provider della mappa del sito viene creata un'istanza la prima volta è possibile accedervi dal `SiteMap` classe e rimane in memoria per la durata dell'applicazione web. Perché è presente solo un'istanza del provider di mappa del sito che possono essere richiamate dalle più, i visitatori del sito web simultanee, è fondamentale che i metodi i provider siano *thread-safe*.

Per motivi di prestazioni e scalabilità, è importante che il sito in memoria nella cache struttura della mappa e restituire questo presenti nella cache struttura anziché averlo ricreato ogni volta che il `BuildSiteMap` metodo viene richiamato. `BuildSiteMap` può essere chiamato più volte per ogni richiesta di pagina per ogni utente, in base i controlli di spostamento sulla pagina e la profondità della struttura della mappa del sito in uso. In qualsiasi caso, se non si memorizza nella cache della struttura della mappa del sito in `BuildSiteMap` ogni volta che viene richiamato è necessario recuperare di nuovo le informazioni product e category dall'architettura (con la conseguenza di una query al database). Come descritto nelle esercitazioni precedenti la memorizzazione nella cache, dati memorizzati nella cache possono diventare non aggiornati. Per evitare questo inconveniente, è possibile usare ora - o oggetti scaduti di basato sulla dipendenza della cache SQL.

> [!NOTE]
> Un provider di mappa del sito può facoltativamente eseguire l'override di [ `Initialize` metodo](https://msdn.microsoft.com/library/system.web.sitemapprovider.initialize.aspx). `Initialize` viene richiamato quando il provider della mappa del sito viene innanzitutto creata un'istanza e viene passato eventuali attributi personalizzati assegnati al provider nel `Web.config` nella `<add>` elemento, ad esempio: `<add name="name" type="type" customAttribute="value" />`. È utile se si desidera consentire a uno sviluppatore di pagina specificare diverse impostazioni correlate al provider della mappa del sito senza dover modificare il codice del provider s. Ad esempio, se si stanno leggendo i dati di categoria e prodotti direttamente dal database anziché tramite l'architettura, è il d probabilmente necessario consentire allo sviluppatore di specificare la stringa di connessione del database tramite `Web.config` invece di usare un livello di codice valore nel codice del provider s. Il provider della mappa del sito personalizzato che verrà creata nel passaggio 6 non eseguirne l'override `Initialize` (metodo). Per un esempio d'uso di `Initialize` metodo, fare riferimento a [Jeff Prosise](http://www.wintellect.com/Weblogs/CategoryView,category,Jeff%20Prosise.aspx) s [l'archiviazione delle mappe dei siti in SQL Server](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) articolo.


## <a name="step-6-creating-the-custom-site-map-provider"></a>Passaggio 6: Creazione di Provider della mappa del sito personalizzato

Per creare un provider della mappa del sito personalizzato che si basa la mappa del sito dalle categorie e i prodotti nel database Northwind, è necessario creare una classe che estende `StaticSiteMapProvider`. Nel passaggio 1 chiede di aggiungere un `CustomProviders` cartella di `App_Code` cartella - aggiungere una nuova classe in questa cartella denominata `NorthwindSiteMapProvider`. Aggiungere il codice seguente alla classe `NorthwindSiteMapProvider` :


[!code-csharp[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample6.cs)]

Let s per iniziare l'esplorazione di questa classe s `BuildSiteMap` metodo, che inizia con un [ `lock` istruzione](https://msdn.microsoft.com/library/c5kehkcz.aspx). Il `lock` istruzione consente solo un thread alla volta per immettere, in tal modo la serializzazione dell'accesso al relativo codice e impediscono l'esecuzione di istruzioni in piedi s tra loro due thread simultanei.

Livello di classe `SiteMapNode` variabile `root` viene usato per memorizzare nella cache della struttura della mappa del sito. Quando viene costruita la mappa del sito per la prima volta oppure per la prima volta dopo che i dati sottostanti sono stati modificati, `root` saranno `null` e verrà creata la struttura della mappa del sito. Il nodo radice della mappa s sito viene assegnato a `root` durante la costruzione processo in modo che alla successiva esecuzione di questo metodo viene chiamato, `root` non saranno `null`. Di conseguenza, purché `root` non è `null` struttura della mappa del sito verrà restituita al chiamante senza dover ricreare l'oggetto.

Se è radice `null`, la struttura della mappa del sito viene creata dalle informazioni product e category. Mappa del sito viene compilata mediante la creazione di `SiteMapNode` istanze e che quindi costituiscono la gerarchia mediante chiamate al `StaticSiteMapProvider` classe s `AddNode` (metodo). `AddNode` esegue la gestione interna, l'archiviazione di vengono formattati diversi `SiteMapNode` di istanze in un `Hashtable`. Prima di iniziare la costruzione della gerarchia, si inizia chiamando il `Clear` metodo, che cancella gli elementi dall'interno `Hashtable`. Successivamente, il `ProductsBLL` classe s `GetProducts` metodo e risultante `ProductsDataTable` vengono archiviati in variabili locali.

La creazione delle mappe s sito inizia con la creazione del nodo radice e assegnarlo alla `root`. L'overload dei metodi di [ `SiteMapNode` costruttore s](https://msdn.microsoft.com/library/system.web.sitemapnode.sitemapnode.aspx) usato qui e in tutto il `BuildSiteMap` viene passato le informazioni seguenti:

- Un riferimento al provider di mappa del sito (`this`).
- Il `SiteMapNode` s `Key`. Questa operazione necessaria deve essere univoco per ogni valore `SiteMapNode`.
- Il `SiteMapNode` s `Url`. `Url` è facoltativo, tuttavia, se fornito, ognuna `SiteMapNode` s `Url` valore deve essere univoco.
- Il `SiteMapNode` s `Title`, che è necessario.

Il `AddNode(root)` chiamata metodo aggiunge le `SiteMapNode` `root` alla mappa del sito come radice. Successivamente, ogni `ProductRow` nella `ProductsDataTable` viene enumerata. Se esiste già un `SiteMapNode` per la categoria di prodotto s corrente, vi viene fatto riferimento. In caso contrario, una nuova `SiteMapNode` per la categoria viene creata e aggiunto come figlio di `SiteMapNode``root` tramite il `AddNode(categoryNode, root)` chiamata al metodo. Dopo la categoria appropriata `SiteMapNode` nodo è stato trovato o creato, un `SiteMapNode` viene creato per il prodotto corrente e aggiunto come elemento figlio della categoria `SiteMapNode` tramite `AddNode(productNode, categoryNode)`. Si noti che la categoria `SiteMapNode` s `Url` valore della proprietà `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID=categoryID` mentre il prodotto `SiteMapNode` s `Url` proprietà viene assegnato `~/SiteMapNode/ProductDetails.aspx?ProductID=productID`.

> [!NOTE]
> I prodotti con un database `NULL` valore per la loro `CategoryID` vengono raggruppate in una categoria `SiteMapNode` cui `Title` è impostata su None e il cui `Url` viene impostata su una stringa vuota. È deciso di adottare `Url` su una stringa vuota dopo il `ProductBLL` classe s `GetProductsByCategory(categoryID)` metodo attualmente non è in grado di restituire solo i prodotti con un `NULL` `CategoryID` valore. Inoltre, vorrei illustrare come i controlli di spostamento il rendering un' `SiteMapNode` che non è presente un valore per la relativa `Url` proprietà. Ti invitiamo a estendere questa esercitazione in modo che nessun `SiteMapNode` s `Url` proprietà fa riferimento alla `ProductsByCategory.aspx`, ancora vengono visualizzati solo i prodotti `NULL` `CategoryID` valori.


Al termine della creazione della mappa del sito, un oggetto arbitrario viene aggiunto alla cache dei dati con una dipendenza della cache SQL nel `Categories` e `Products` tabelle tramite un `AggregateCacheDependency` oggetto. Sono stati presentati tramite le dipendenze della cache SQL nell'esercitazione precedente *usando le dipendenze della Cache di SQL*. Il provider della mappa del sito personalizzato, tuttavia, Usa un overload della cache dei dati s `Insert` metodo che ve ancora a esplorare. Questo overload accetta come parametro di input finale un delegato che viene chiamato quando l'oggetto viene rimosso dalla cache. In particolare, vengono passati in una nuova [ `CacheItemRemovedCallback` delegare](https://msdn.microsoft.com/library/system.web.caching.cacheitemremovedcallback.aspx) che punta al `OnSiteMapChanged` metodo definito più avanti nel `NorthwindSiteMapProvider` classe.

> [!NOTE]
> La rappresentazione in memoria della mappa del sito viene memorizzato nella cache tramite la variabile a livello di classe `root`. Perché è presente solo un'istanza della classe del provider di mappa del sito personalizzato e dato che tale istanza viene condiviso tra tutti i thread nell'applicazione web, questa variabile di classe funge da una cache. Il `BuildSiteMap` metodo Usa anche la cache dei dati, ma solo come mezzo per ricevere una notifica quando sottostante di dati nel database di `Categories` o `Products` le tabelle delle modifiche. Si noti che il valore inserito nella cache di dati è sufficiente la data e ora correnti. Dati della mappa del sito effettivo sono *non* inserire nella cache dei dati.


Il `BuildSiteMap` metodo viene completato, restituendo il nodo radice della mappa del sito.

I metodi rimanenti sono piuttosto semplici. `GetRootNodeCore` è responsabile della restituzione del nodo radice. Poiché `BuildSiteMap` restituisce la radice `GetRootNodeCore` restituisce semplicemente `BuildSiteMap` s valore restituito. Il `OnSiteMapChanged` metodo imposta `root` al `null` quando viene rimosso l'elemento della cache. Con radice reimpostato `null`, all'accesso successivo `BuildSiteMap` viene richiamato, la struttura della mappa del sito verrà ricostruita. Infine, il `CachedDate` proprietà restituisce il valore di data e ora archiviato nella cache dei dati, se esiste un valore di questo tipo. Questa proprietà può essere utilizzata dagli sviluppatori di pagine per determinare quando è stato ultima cache per i dati della mappa del sito.

## <a name="step-7-registering-thenorthwindsitemapprovider"></a>Passaggio 7: Registrazione di`NorthwindSiteMapProvider`

Affinché l'applicazione web per usare la `NorthwindSiteMapProvider` provider di mappa del sito creato nel passaggio 6, è necessario registrarlo nel `<siteMap>` sezione del `Web.config`. In particolare, aggiungere il markup seguente all'interno di `<system.web>` elemento `Web.config`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample7.xml)]

Questo markup fa due cose: in primo luogo, indica che l'elemento predefinito `AspNetXmlSiteMapProvider` il provider della mappa del sito predefinito; in secondo luogo, registra il provider della mappa del sito personalizzato creato nel passaggio 6 con il nome di Converto Northwind.

> [!NOTE]
> Per i provider della mappa del sito che si trova in s application `App_Code` cartella, il valore della `type` attributo è semplicemente il nome della classe. In alternativa, il provider della mappa del sito personalizzato potrebbe essere stato creato in un progetto libreria di classi separato con l'assembly compilato posizionato in s dell'applicazione web `/Bin` directory. In tal caso, il `type` valore dell'attributo sarebbe *Namespace*. *ClassName*, *nomeassembly* .


Dopo aver aggiornato `Web.config`, si consiglia di visualizzare qualsiasi pagina dalle esercitazioni in un browser. Si noti che l'interfaccia di spostamento a sinistra viene ancora visualizzato le sezioni e le esercitazioni definito in `Web.sitemap`. Infatti, è stato lasciato `AspNetXmlSiteMapProvider` come provider predefinito. Per creare un elemento dell'interfaccia utente di navigazione che utilizza il `NorthwindSiteMapProvider`, sarà necessario specificare esplicitamente che il provider della mappa del sito Northwind deve essere utilizzato. Si vedrà come eseguire questa operazione nel passaggio 8.

## <a name="step-8-displaying-site-map-information-using-the-custom-site-map-provider"></a>Passaggio 8: Visualizzazione di informazioni relative alla mappa del sito usando il Provider della mappa del sito personalizzata

Con il sito personalizzato provider della mappa creati e registrati in `Web.config`, sono pronti per aggiungere i controlli di navigazione per il `Default.aspx`, `ProductsByCategory.aspx`, e `ProductDetails.aspx` pagine la `SiteMapProvider` cartella. Iniziare aprendo il `Default.aspx` pagina e trascinare un `SiteMapPath` dalla casella degli strumenti nella finestra di progettazione. Il controllo SiteMapPath si trova nella sezione esplorazione della casella degli strumenti.


[![Aggiungere un SiteMapPath a default. aspx](building-a-custom-database-driven-site-map-provider-cs/_static/image19.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image18.gif)

**Figura 16**: aggiungere un SiteMapPath alla `Default.aspx` ([fare clic per visualizzare l'immagine con dimensioni normali](building-a-custom-database-driven-site-map-provider-cs/_static/image20.gif))


Il controllo SiteMapPath Visualizza una barra di navigazione, che indica la posizione corrente della pagina s all'interno della mappa del sito. Abbiamo aggiunto un SiteMapPath alla parte superiore della pagina master nel *pagine Master e spostamento nel sito* esercitazione.

Richiedere qualche istante per visualizzare questa pagina tramite un browser. Il controllo SiteMapPath aggiunto nella figura 16 Usa il provider di mappa del sito predefinito, estrarre i dati da `Web.sitemap`. Pertanto, il breadcrumb indica casa &gt; personalizzazione mappa del sito, esattamente come il percorso di navigazione nell'angolo superiore destro.


[![Il percorso di navigazione Usa il Provider di mappa del sito predefinito](building-a-custom-database-driven-site-map-provider-cs/_static/image22.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image21.gif)

**Figura 17**: il percorso di navigazione Usa il Provider della mappa del sito predefinito ([fare clic per visualizzare l'immagine con dimensioni normali](building-a-custom-database-driven-site-map-provider-cs/_static/image23.gif))


Per consentire il controllo SiteMapPath aggiunto nella figura 16 a usare il provider di mappa del sito personalizzato creato nel passaggio 6, impostare relativi [ `SiteMapProvider` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemappath.sitemapprovider.aspx) a Northwind, il nome viene assegnato al `NorthwindSiteMapProvider` in `Web.config`. Sfortunatamente, la finestra di progettazione continua a usare il provider di mappa del sito predefinito, ma se si visita la pagina tramite un browser dopo aver apportato questa modifica di proprietà si noterà che il percorso di navigazione ora Usa il provider della mappa del sito personalizzata.


[![Il percorso di navigazione Usa ora la NorthwindSiteMapProvider Provider della mappa del sito personalizzata](building-a-custom-database-driven-site-map-provider-cs/_static/image25.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image24.gif)

**Figura 18**: il percorso di navigazione ora Usa il Provider della mappa del sito personalizzato `NorthwindSiteMapProvider` ([fare clic per visualizzare l'immagine con dimensioni normali](building-a-custom-database-driven-site-map-provider-cs/_static/image26.gif))


Il controllo SiteMapPath Visualizza un'interfaccia utente più funzionale nel `ProductsByCategory.aspx` e `ProductDetails.aspx` pagine. Aggiungere un SiteMapPath per queste pagine, l'impostazione di `SiteMapProvider` proprietà sia a Northwind. Da `Default.aspx` fare clic sul collegamento Visualizza prodotti relativi alle bevande, quindi sul collegamento Visualizza dettagli per Chai tè. Come illustrato nella figura 19, la barra di navigazione include la sezione di mappa del sito corrente (Chai tè) e i relativi predecessori: Beverages e tutte le categorie.


[![Il percorso di navigazione Usa ora la NorthwindSiteMapProvider Provider della mappa del sito personalizzata](building-a-custom-database-driven-site-map-provider-cs/_static/image27.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image21.png)

**Figura 19**: il percorso di navigazione ora Usa il Provider della mappa del sito personalizzato `NorthwindSiteMapProvider` ([fare clic per visualizzare l'immagine con dimensioni normali](building-a-custom-database-driven-site-map-provider-cs/_static/image22.png))


Altri elementi dell'interfaccia utente di navigazione sono utilizzabile oltre il controllo SiteMapPath, ad esempio i controlli Menu e TreeView. Il `Default.aspx`, `ProductsByCategory.aspx`, e `ProductDetails.aspx` pagine di download per questa esercitazione, ad esempio, includono i controlli Menu (vedere Figura 20). Vedere [analisi ASP.NET 2.0 s le funzionalità di spostamento sito](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx) e il [usando i controlli di spostamento sito](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/navigation/sitenavcontrols.aspx) sezione del [guide introduttive di ASP.NET 2.0](https://quickstarts.asp.net/QuickStartv20/aspnet/) per un esame più approfondito il controlli di spostamento e il sistema di mappa del sito in ASP.NET 2.0.


[![Il controllo Menu riporta tutte le categorie e i prodotti](building-a-custom-database-driven-site-map-provider-cs/_static/image29.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image28.gif)

**Figura 20**: il Menu di controllo Elenca ogni e delle categorie prodotti ([fare clic per visualizzare l'immagine con dimensioni normali](building-a-custom-database-driven-site-map-provider-cs/_static/image30.gif))


Come accennato in precedenza in questa esercitazione, la struttura della mappa del sito sono accessibili a livello di programmazione tramite le `SiteMap` classe. Il codice seguente restituisce la radice `SiteMapNode` del provider predefinito:


[!code-csharp[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample8.cs)]

Poiché il `AspNetXmlSiteMapProvider` è il provider predefinito per la nostra applicazione, il codice precedente restituisce il nodo radice definito in `Web.sitemap`. Per fare riferimento a un provider di mappa del sito diverso da quello predefinito, usare il `SiteMap` classe s [ `Providers` proprietà](https://msdn.microsoft.com/library/system.web.sitemap.providers.aspx) come segue:


[!code-csharp[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample9.cs)]

In cui *nome* è il nome del provider della mappa del sito personalizzato (per la nostra applicazione web, Northwind).

Per accedere a un membro specifico per un provider di mappa del sito, usare `SiteMap.Providers["name"]` per recuperare l'istanza del provider e quindi eseguire il cast nel tipo appropriato. Ad esempio, per visualizzare il `NorthwindSiteMapProvider` s `CachedDate` proprietà in una pagina ASP.NET, usare il codice seguente:


[!code-csharp[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample10.cs)]

> [!NOTE]
> Assicurarsi di testare la funzionalità di dipendenza della cache SQL. Dopo aver visitare il `Default.aspx`, `ProductsByCategory.aspx`, e `ProductDetails.aspx` pagine, passare a una delle esercitazioni di modifica, inserimento ed eliminazione di sezione e modificare il nome di una categoria o un prodotto. Quindi tornare a una delle pagine di `SiteMapProvider` cartella. Supponendo che sia trascorso tempo sufficiente per il meccanismo di polling annotare la modifica al database sottostante, la mappa del sito deve essere aggiornata per mostrare il nuovo prodotto o il nome di categoria.


## <a name="summary"></a>Riepilogo

ASP.NET 2.0 include le funzionalità di mappa del sito s un `SiteMap` (classe), un numero di navigazione predefinito Web controlla e salvati in modo permanente un provider della mappa del sito predefinito che prevede che le informazioni della mappa del sito in un file XML. Per usare le informazioni sulla mappa del sito da un'altra origine, ad esempio da un database, l'architettura s dell'applicazione o un servizio Web remoto, che è necessario creare un provider della mappa del sito personalizzata. Ciò comporta la creazione di una classe derivata, in modo diretto o indiretto, dal `SiteMapProvider` classe.

In questa esercitazione è stato illustrato come creare un provider di mappa del sito personalizzato che basa la mappa del sito sulle informazioni product e category raccolti dall'architettura dell'applicazione. Il provider di servizi estesi il `StaticSiteMapProvider` classe e la conseguente creazione di un `BuildSiteMap` metodo di recuperata dei dati, costruito gerarchia della mappa del sito e memorizzati nella cache la struttura risultante in una variabile a livello di classe. Abbiamo utilizzato una dipendenza della cache SQL con una funzione di callback per invalidare memorizzato nella cache quando struttura sottostante `Categories` o `Products` vengono modificati i dati.

Buona programmazione!

## <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [L'archiviazione delle mappe dei siti in SQL Server](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) e [SQL Site Map Provider è già stato di attesa per](https://msdn.microsoft.com/msdnmag/issues/06/02/wickedcode/default.aspx)
- [Modello Provider s un'occhiata a ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)
- [Il Toolkit del Provider](https://msdn.microsoft.com/asp.net/aa336558.aspx)
- [Analisi di ASP.NET 2.0 le funzionalità di spostamento sito s](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. I revisori per questa esercitazione sono state Dave Gardner, Zack Jones, Teresa Murphy e Bernadette Leigh. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [avanti](building-a-custom-database-driven-site-map-provider-vb.md)
