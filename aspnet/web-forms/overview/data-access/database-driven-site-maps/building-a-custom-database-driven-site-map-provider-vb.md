---
uid: web-forms/overview/data-access/database-driven-site-maps/building-a-custom-database-driven-site-map-provider-vb
title: Creazione di un Provider di mappa del sito basati su Database personalizzato (VB) | Documenti Microsoft
author: rick-anderson
description: Il provider di mappa del sito predefinito in ASP.NET 2.0 recupera i dati da un file XML statico. Mentre il provider basato su XML sia adatto a molte aziende di piccole e medie finestra...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: f904cd2c-a408-4484-9324-8b8d7fe33893
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/database-driven-site-maps/building-a-custom-database-driven-site-map-provider-vb
msc.type: authoredcontent
ms.openlocfilehash: f886936c0033c9fac9c81fe8d2f7905228a9867d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="building-a-custom-database-driven-site-map-provider-vb"></a>Creazione di un Provider di mappa del sito basati su Database personalizzato (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare codice](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_62_VB.zip) o [Scarica il PDF](building-a-custom-database-driven-site-map-provider-vb/_static/datatutorial62vb1.pdf)

> Il provider di mappa del sito predefinito in ASP.NET 2.0 recupera i dati da un file XML statico. Mentre il provider basato su XML è adatto a molte i siti Web di piccoli e medie dimensioni, le applicazioni Web più grandi richiedono una mappa del sito più dinamica. In questa esercitazione che si creerà un provider di mappe del sito personalizzato che recupera i dati dal livello di logica di Business, che a sua volta recupera dati dal database.


## <a name="introduction"></a>Introduzione

ASP.NET 2.0 funzionalità di mappa del sito s consente a uno sviluppatore di pagina definire una mappa del sito di s applicazione web in un supporto permanente, ad esempio in un file XML. Una volta definito, i dati della mappa del sito è possibile accedere a livello di programmazione tramite le [ `SiteMap` classe](https://msdn.microsoft.com/en-us/library/system.web.sitemap.aspx) nel [ `System.Web` dello spazio dei nomi](https://msdn.microsoft.com/en-us/library/system.web.aspx) o attraverso una serie di navigazione Web, controlli, ad esempio il Controlli SiteMapPath, Menu e TreeView. Il sistema utilizza il [modello provider](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx) in modo che le implementazioni di serializzazione della mappa del sito diversi possono essere create e collegate in un'applicazione web. Il provider della mappa del sito predefinito fornito con ASP.NET 2.0 persiste struttura della mappa del sito in un file XML. Nel [pagine Master e esplorazione del sito](../introduction/master-pages-and-site-navigation-vb.md) esercitazione è stato creato un file denominato `Web.sitemap` che contenuti di questa struttura e di essere stato aggiornato il codice XML con ogni nuova sezione dell'esercitazione.

Il provider di mappa del sito basati su XML predefinito funziona anche se la struttura di s mappa del sito è abbastanza statica, ad esempio nelle esercitazioni. In molti scenari, tuttavia, è necessaria una mappa del sito più dinamica. Prendere in considerazione la mappa del sito illustrata nella figura 1, dove ogni categoria e prodotto vengono visualizzati come sezioni della struttura s sito Web. Con questa mappa del sito, visitare la pagina web corrispondente al nodo radice potrebbe elencare tutte le categorie, mentre visitare una pagina web particolare categoria s vengono elencati i prodotti s tale categoria e la visualizzazione di una pagina web di un prodotto specifico s avrebbe prodotto s-dettagli.


[![Le categorie e i prodotti composizione struttura s mappa del sito](building-a-custom-database-driven-site-map-provider-vb/_static/image1.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image1.png)

**Figura 1**: di categorie e prodotti composizione struttura s mappa del sito ([fare clic per visualizzare l'immagine ingrandita](building-a-custom-database-driven-site-map-provider-vb/_static/image2.png))


Durante questa struttura di base a categoria e prodotto potrebbe essere hard-coded nel `Web.sitemap` file, il file dovrà essere aggiornata ogni volta che una categoria o prodotto è stato aggiunto, rimosso o rinominato. Di conseguenza, la manutenzione delle mappe del sito potrebbe essere notevolmente semplificata se la struttura è stata recuperata dal database di o, in teoria, dal livello di logica di Business dell'architettura s dell'applicazione. In questo modo, come i prodotti e le categorie sono state aggiunte, rinominato o eliminato, la mappa del sito consente l'aggiornamento automatico per riflettere queste modifiche.

Poiché la serializzazione della mappa del sito di ASP.NET 2.0 s viene compilata nella parte superiore del modello di provider, è possibile creare un provider di mappe di sito personalizzato che acquisisce i dati da un archivio di dati alternativi, ad esempio il database o l'architettura. In questa esercitazione creeremo un provider personalizzato che recupera i dati da BLL. Let s iniziare!

> [!NOTE]
> Il provider della mappa del sito personalizzato creato in questa esercitazione è strettamente associato al modello di applicazione s architettura e i dati. Jeff Prosise s [l'archiviazione delle mappe del sito in SQL Server](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) e [il Provider di mappa del sito SQL è già stato di attesa per](https://msdn.microsoft.com/msdnmag/issues/06/02/wickedcode/default.aspx) articoli esaminare un approccio generalizzato per l'archiviazione di dati della mappa del sito in SQL Server.


## <a name="step-1-creating-the-custom-site-map-provider-web-pages"></a>Passaggio 1: Creazione delle pagine Web Provider mappa del sito personalizzato

Prima di iniziare la creazione di un provider di mappe del sito personalizzato, consentire s prima di aggiungere le pagine ASP.NET che è necessario per questa esercitazione. Per iniziare, aggiungere una nuova cartella denominata `SiteMapProvider`. Successivamente, aggiungere le seguenti pagine ASP.NET a quella cartella, assicurandosi di associare ogni pagina con il `Site.master` pagina master:

- `Default.aspx`
- `ProductsByCategory.aspx`
- `ProductDetails.aspx`

Aggiungere anche un `CustomProviders` sottocartella di `App_Code` cartella.


![Aggiungere le pagine ASP.NET per le esercitazioni di correlate al Provider della mappa del sito](building-a-custom-database-driven-site-map-provider-vb/_static/image2.gif)

**Figura 2**: aggiungere le pagine ASP.NET per il sito di eseguire il mapping di esercitazioni relative a Provider


Perché è presente un solo esercitazione di questa sezione, non abbiamo bisogno di t `Default.aspx` per elencare le esercitazioni di sezione s. In alternativa, `Default.aspx` visualizzerà le categorie in un controllo GridView. Che verranno affrontati in questo passaggio 2.

Aggiornare quindi `Web.sitemap` per includere un riferimento di `Default.aspx` pagina. In particolare, aggiungere il markup seguente dopo la memorizzazione nella cache `<siteMapNode>`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample1.xml)]

Dopo aver aggiornato `Web.sitemap`, dedicare alcuni minuti per visualizzare il sito Web esercitazioni tramite un browser. Il menu a sinistra ora include un elemento per l'esercitazione di provider di mappa unico sito.


![Mappa del sito include ora una voce per l'esercitazione di Provider di mappa del sito](building-a-custom-database-driven-site-map-provider-vb/_static/image3.gif)

**Figura 3**: mappa del sito include ora una voce per l'esercitazione di Provider di mappa del sito


L'obiettivo principale di esercitazione s è per illustrare la creazione di un provider di mappe personalizzate del sito e configurazione di un'applicazione web per utilizzare tale provider. In particolare, verrà creata un provider che restituisce una mappa del sito che include un nodo radice con un nodo per ogni categoria e il prodotto, come illustrato nella figura 1. In generale, ogni nodo nella mappa del sito può specificare un URL. Per la mappa del sito, sarà l'URL radice del nodo s `~/SiteMapProvider/Default.aspx`, che elenca tutte le categorie nel database. Ogni nodo della categoria nella mappa del sito avrà un URL che punta a `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID=categoryID`, che verranno elencati tutti i prodotti nell'oggetto specificato *categoryID*. Infine, ogni nodo della mappa del sito di prodotto punterà al valore `~/SiteMapProvider/ProductDetails.aspx?ProductID=productID`, che verranno visualizzati i dettagli di un prodotto specifico s.

Per iniziare è necessario creare il `Default.aspx`, `ProductsByCategory.aspx`, e `ProductDetails.aspx` pagine. Queste pagine vengono completate nei passaggi 2, 3 e 4, rispettivamente. Poiché la di questa esercitazione è dedicata al provider della mappa, poiché sono trattate esercitazioni precedenti la creazione di questi tipi di più pagine master-details report e, si verrà Affrettati tramite i passaggi 2 e 4. Se è necessario un aggiornamento sulla creazione di report master/dettaglio che si estendono su più pagine, fare riferimento in futuro il [Master-Details filtro tra due pagine](../masterdetail/master-detail-filtering-across-two-pages-vb.md) esercitazione.

## <a name="step-2-displaying-a-list-of-categories"></a>Passaggio 2: Visualizzare un elenco di categorie

Aprire il `Default.aspx` nella pagina di `SiteMapProvider` cartelle e trascinare un controllo GridView dalla casella degli strumenti nella finestra di progettazione, l'impostazione relativa `ID` per `Categories`. Il GridView s smart tag, associarlo a un nuovo oggetto ObjectDataSource denominato `CategoriesDataSource` e configurarlo in modo che recuperi relativi dati tramite il `CategoriesBLL` classe s `GetCategories` (metodo). Poiché questo GridView semplicemente visualizza le categorie e non fornisce funzionalità di modifica dei dati, impostare gli elenchi a discesa nell'aggiornamento, inserimento ed eliminare le schede su (nessuno).


[![Configurare ObjectDataSource per restituire le categorie utilizzando il metodo GetCategories](building-a-custom-database-driven-site-map-provider-vb/_static/image4.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image3.png)

**Figura 4**: configurare ObjectDataSource all'utilizzo di categorie di restituire il `GetCategories` metodo ([fare clic per visualizzare l'immagine ingrandita](building-a-custom-database-driven-site-map-provider-vb/_static/image4.png))


[![Impostare gli elenchi a discesa nell'aggiornamento, inserimento ed eliminare le schede su (nessuno)](building-a-custom-database-driven-site-map-provider-vb/_static/image5.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image5.png)

**Figura 5**: impostare l'elenco a discesa sono elencati nelle schede DELETE, INSERT e UPDATE su (nessuno) ([fare clic per visualizzare l'immagine ingrandita](building-a-custom-database-driven-site-map-provider-vb/_static/image6.png))


Dopo aver completato la configurazione guidata origine dati, Visual Studio aggiungerà un BoundField per `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`, e `BrochurePath`. Modificare il controllo GridView in modo che contenga solo il `CategoryName` e `Description` BoundField e aggiornare il `CategoryName` BoundField s `HeaderText` proprietà per categoria.

Successivamente, aggiungere un HyperLinkField e posizionarlo in modo che il campo più a sinistra s. Impostare il `DataNavigateUrlFields` proprietà `CategoryID` e `DataNavigateUrlFormatString` proprietà `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID={0}`. Impostare il `Text` proprietà per visualizzare i prodotti.


![Aggiungere un HyperLinkField a GridView categorie](building-a-custom-database-driven-site-map-provider-vb/_static/image6.gif)

**Figura 6**: aggiungere un HyperLinkField per il `Categories` GridView


Dopo la creazione di ObjectDataSource e personalizzare i campi di GridView s, il markup dichiarativo due controlli sarà simile al seguente:


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample2.aspx)]

Figura 7 illustra `Default.aspx` quando viene visualizzato tramite un browser. Fare clic su una categoria s Visualizza prodotti collegamento reindirizza alla `ProductsByCategory.aspx?CategoryID=categoryID`, che verrà creata nel passaggio 3.


[![Ogni categoria è elencati insieme con un collegamento di prodotti di visualizzazione](building-a-custom-database-driven-site-map-provider-vb/_static/image7.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image7.png)

**Figura 7**: ogni categoria è elencati insieme con un collegamento di prodotti di visualizzazione ([fare clic per visualizzare l'immagine ingrandita](building-a-custom-database-driven-site-map-provider-vb/_static/image8.png))


## <a name="step-3-listing-the-selected-category-s-products"></a>Passaggio 3: Elenco dei prodotti di s categoria selezionata

Aprire il `ProductsByCategory.aspx` pagina e aggiungere un controllo GridView, denominarla `ProductsByCategory`. Smart tag, di associare un nuovo oggetto ObjectDataSource denominato GridView `ProductsByCategoryDataSource`. Configurare ObjectDataSource per utilizzare il `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` metodo e impostare l'elenco a discesa sono elencati su (nessuno) nelle schede UPDATE, INSERT e DELETE.


[![Utilizzare il metodo di classe ProductsBLL s GetProductsByCategoryID(categoryID)](building-a-custom-database-driven-site-map-provider-vb/_static/image8.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image9.png)

**Figura 8**: utilizzo di `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` metodo ([fare clic per visualizzare l'immagine ingrandita](building-a-custom-database-driven-site-map-provider-vb/_static/image10.png))


Richiede il passaggio finale della procedura guidata Configura origine dati per un'origine di parametro per *categoryID*. Poiché queste informazioni vengono passate tramite il campo querystring `CategoryID`, selezionare QueryString dall'elenco a discesa e immettere CategoryID nella casella di testo QueryStringField, come illustrato nella figura 9. Fare clic su Fine per completare la procedura guidata.


[![Utilizzare il campo Querystring CategoryID per il parametro categoryID](building-a-custom-database-driven-site-map-provider-vb/_static/image9.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image11.png)

**Figura 9**: utilizzo di `CategoryID` Querystring Field per la *categoryID* parametro ([fare clic per visualizzare l'immagine ingrandita](building-a-custom-database-driven-site-map-provider-vb/_static/image12.png))


Dopo aver completato la procedura guidata, Visual Studio aggiungerà BoundField corrispondente e un CheckBoxField a GridView per i campi di dati di prodotto. Rimuovere tutto tranne il `ProductName`, `UnitPrice`, e `SupplierName` BoundField. Personalizzare questi tre BoundField `HeaderText` proprietà leggere prodotto, prezzo e fornitore, rispettivamente. Formato di `UnitPrice` BoundField come valuta.

Successivamente, aggiungere un HyperLinkField e spostarlo nella posizione più a sinistra. Impostare il relativo `Text` proprietà per visualizzare i dettagli, relativi `DataNavigateUrlFields` proprietà `ProductID`e il relativo `DataNavigateUrlFormatString` proprietà `~/SiteMapProvider/ProductDetails.aspx?ProductID={0}`.


![Aggiungere un HyperLinkField dettagli di visualizzazione che punta a ProductDetails.aspx](building-a-custom-database-driven-site-map-provider-vb/_static/image10.gif)

**Figura 10**: aggiungere una visualizzazione dettagli HyperLinkField che punta a`ProductDetails.aspx`


Dopo aver apportato queste personalizzazioni, GridView e ObjectDataSource s dichiarativo sarà simile al seguente:


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample3.aspx)]

Tornare alla visualizzazione `Default.aspx` tramite un browser e fare clic su Visualizza prodotti dei collegamenti per Beverages. L'operazione richiederà di `ProductsByCategory.aspx?CategoryID=1`, la visualizzazione di nomi, prezzi e fornitori dei prodotti del database Northwind che appartengono alla categoria Beverages (vedere Figura 11). È possibile migliorare ulteriormente questa pagina per includere un collegamento per restituire gli utenti alla pagina di elenco della categoria (`Default.aspx`) e un controllo DetailsView o FormView che visualizza il nome della categoria selezionata s e la descrizione.


[![Vengono visualizzati i nomi delle bevande, prezzi e fornitori](building-a-custom-database-driven-site-map-provider-vb/_static/image11.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image13.png)

**Figura 11**: vengono visualizzati il nomi bevande, prezzi e fornitori ([fare clic per visualizzare l'immagine ingrandita](building-a-custom-database-driven-site-map-provider-vb/_static/image14.png))


## <a name="step-4-showing-a-product-s-details"></a>Passaggio 4: Visualizzare una s Product Details

Pagina finale, `ProductDetails.aspx`, vengono visualizzati i dettagli di prodotti selezionati. Aprire `ProductDetails.aspx` e trascinare un controllo DetailsView dalla casella degli strumenti nella finestra di progettazione. Impostare i dispositivi di DetailsView `ID` proprietà `ProductInfo` e cancellare il `Height` e `Width` i valori delle proprietà. Smart tag, di associare il controllo DetailsView un nuovo oggetto ObjectDataSource denominato `ProductDataSource`, ObjectDataSource per estrarre i dati di configurazione di `ProductsBLL` classe s `GetProductByProductID(productID)` metodo. Come con le pagine web precedente creato nei passaggi 2 e 3, impostare gli elenchi a discesa nell'aggiornamento, inserimento ed eliminare le schede su (nessuno).


[![Configurare ObjectDataSource per utilizzare il metodo GetProductByProductID(productID)](building-a-custom-database-driven-site-map-provider-vb/_static/image12.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image15.png)

**Figura 12**: configurare ObjectDataSource per utilizzare il `GetProductByProductID(productID)` metodo ([fare clic per visualizzare l'immagine ingrandita](building-a-custom-database-driven-site-map-provider-vb/_static/image16.png))


Richiede l'ultimo passaggio della procedura guidata Configura origine dati per l'origine di *productID* parametro. Poiché questi dati provengono da campo querystring `ProductID`, impostare l'elenco a discesa di stringa di query e la casella di testo di QueryStringField ProductID. Infine, fare clic sul pulsante Fine per completare la procedura guidata.


[![Configurare il parametro per estrarre il valore del campo Querystring ProductID productID](building-a-custom-database-driven-site-map-provider-vb/_static/image13.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image17.png)

**Figura 13**: configurare il *productID* parametro per estrarre il valore di `ProductID` Querystring Field ([fare clic per visualizzare l'immagine ingrandita](building-a-custom-database-driven-site-map-provider-vb/_static/image18.png))


Dopo aver completato la configurazione guidata origine dati, Visual Studio creerà BoundField corrispondente e un CheckBoxField nel controllo DetailsView. per i campi di dati di prodotto. Rimuovere il `ProductID`, `SupplierID`, e `CategoryID` BoundField e configurare i campi rimanenti nel modo desiderato. Dopo un numero limitato di configurazioni estetici, il markup dichiarativo s DetailsView e ObjectDataSource simile al seguente:


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample4.aspx)]

Per testare questa pagina, tornare a `Default.aspx` e fare clic su Visualizza i prodotti per la categoria Beverages. Nell'elenco di prodotti delle bibite, fare clic sul collegamento Visualizza dettagli per Chai tè. L'operazione richiederà di `ProductDetails.aspx?ProductID=1`, s Chai tè che mostra i dettagli (vedere Figura 14).


[![Viene visualizzato Chai tè s fornitore, categoria, prezzo e altre informazioni](building-a-custom-database-driven-site-map-provider-vb/_static/image14.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image19.png)

**Nella figura 14**: viene visualizzato Chai tè s fornitore, categoria, prezzo e altre informazioni ([fare clic per visualizzare l'immagine ingrandita](building-a-custom-database-driven-site-map-provider-vb/_static/image20.png))


## <a name="step-5-understanding-the-inner-workings-of-a-site-map-provider"></a>Passaggio 5: Informazioni sui meccanismi interni di un Provider di mappe del sito

Mappa del sito è rappresentata nella memoria del server s web come una raccolta di `SiteMapNode` istanze che formano una gerarchia. Deve essere presente esattamente un radice, tutti i nodi radice non devono avere esattamente un nodo padre e tutti i nodi possono avere un numero arbitrario di elementi figlio. Ogni `SiteMapNode` oggetto rappresenta una sezione nella struttura del sito Web s; queste sezioni sono in genere una pagina web corrispondente. Di conseguenza, il [ `SiteMapNode` classe](https://msdn.microsoft.com/en-us/library/system.web.sitemapnode.aspx) dispone di proprietà quali `Title`, `Url`, e `Description`, che forniscono informazioni per la sezione di `SiteMapNode` rappresenta. È inoltre disponibile un `Key` proprietà che identifica in modo univoco ogni `SiteMapNode` nella gerarchia, nonché le proprietà utilizzate per stabilire questa gerarchia `ChildNodes`, `ParentNode`, `NextSibling`, `PreviousSibling`e così via.

Figura 15 mostra la struttura della mappa del sito generale dalla figura 1, ma con i dettagli di implementazione stabiliti in maggiore dettaglio.


[![Ogni SiteMapNode è di proprietà come titolo, Url, chiave e così via](building-a-custom-database-driven-site-map-provider-vb/_static/image16.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image15.gif)

**Figura 15**: ogni `SiteMapNode` dispone di proprietà come `Title`, `Url`, `Key`e così via ([fare clic per visualizzare l'immagine ingrandita](building-a-custom-database-driven-site-map-provider-vb/_static/image17.gif))


Mappa del sito è possibile accedere tramite il [ `SiteMap` classe](https://msdn.microsoft.com/en-us/library/system.web.sitemap.aspx) nel [ `System.Web` dello spazio dei nomi](https://msdn.microsoft.com/en-us/library/system.web.aspx). Questa classe s `RootNode` proprietà restituisce la radice della mappa s sito `SiteMapNode` esempio. `CurrentNode` restituisce il `SiteMapNode` cui `Url` proprietà corrisponde all'URL della pagina attualmente richiesta. Questa classe viene utilizzata internamente dai controlli Web di ASP.NET 2.0 s navigazione.

Quando il `SiteMap` accesso alle proprietà di classe s, è necessario serializzare la struttura della mappa del sito da un supporto permanente in memoria. Tuttavia, la logica di serializzazione di mappa del sito non è rigida codificate la `SiteMap` classe. In alternativa, in fase di esecuzione di `SiteMap` classe determina quali mappa del sito *provider* da utilizzare per la serializzazione. Per impostazione predefinita, il [ `XmlSiteMapProvider` classe](https://msdn.microsoft.com/en-us/library/system.web.xmlsitemapprovider.aspx) viene utilizzata la struttura s mappa del sito per la lettura da un file XML formattato correttamente. Tuttavia, con un minimo di lavoro è possibile creare provider di mappe nostro sito personalizzato.

Tutti i provider di mappa del sito devono essere derivati dal [ `SiteMapProvider` classe](https://msdn.microsoft.com/en-us/library/system.web.sitemapprovider.aspx), che include i metodi essenziali e le proprietà necessarie per il sito provider della mappa, ma omette molti dettagli dell'implementazione. Una seconda classe, [ `StaticSiteMapProvider` ](https://msdn.microsoft.com/en-us/library/system.web.staticsitemapprovider.aspx), estende la `SiteMapProvider` classe e contiene un'implementazione più affidabile di funzionalità necessarie. Internamente, il `StaticSiteMapProvider` archivia il `SiteMapNode` eseguire il mapping di istanze del sito un `Hashtable` e fornisce metodi come `AddNode(child, parent)`, `RemoveNode(siteMapNode),` e `Clear()` che aggiungere e rimuovere `SiteMapNode` s alla classe interna `Hashtable`. L'oggetto `XmlSiteMapProvider` è derivato da `StaticSiteMapProvider`.

Quando la creazione di un provider di mappe del sito personalizzata che estende `StaticSiteMapProvider`, sono disponibili due metodi astratti che devono essere sottoposto a override: [ `BuildSiteMap` ](https://msdn.microsoft.com/en-us/library/system.web.staticsitemapprovider.buildsitemap.aspx) e [ `GetRootNodeCore` ](https://msdn.microsoft.com/en-us/library/system.web.sitemapprovider.getrootnodecore.aspx). `BuildSiteMap`, come suggerisce il nome, è responsabile del caricamento della struttura della mappa del sito dall'archivio permanente e la creazione in memoria. `GetRootNodeCore`Restituisce il nodo radice nella mappa del sito.

Prima di un sito web l'applicazione può utilizzare un provider di mappe del sito che deve essere registrato nella configurazione s dell'applicazione. Per impostazione predefinita, il `XmlSiteMapProvider` classe registrata utilizzando il nome `AspNetXmlSiteMapProvider`. Per registrare i provider della mappa del sito aggiuntivi, aggiungere il markup seguente per `Web.config`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample5.xml)]

Il *nome* valore viene assegnato un nome leggibile del provider durante *tipo* specifica il nome completo del provider di mappa del sito. Si esamineranno valori concreti per il *nome* e *tipo* valori nel passaggio 7, dopo abbiamo creato il provider di mappa del sito personalizzato.

La classe di provider di mappa del sito viene creata la prima volta, è possibile accedervi dalla `SiteMap` classe e rimane in memoria per la durata dell'applicazione web. Perché è presente solo un'istanza del provider di mappa del sito che possono essere richiamati dai visitatori del sito web simultanee più, è fondamentale che i metodi di provider s essere *thread-safe*.

Per motivi di prestazioni e scalabilità, è importante che il sito in memoria nella cache struttura della mappa e restituire questo memorizzati struttura anziché averlo ricreato ogni volta il `BuildSiteMap` metodo viene richiamato. `BuildSiteMap`può essere chiamato più volte per ogni richiesta di pagina per ogni utente, a seconda di controlli per la navigazione in uso nella pagina e la profondità della struttura della mappa del sito. In ogni caso, se non si memorizzano nella cache della struttura della mappa del sito in `BuildSiteMap` viene richiamato ogni volta che è necessario recuperare nuovamente le informazioni relative al prodotto e categoria l'architettura (con la conseguenza di una query al database). Come descritto nelle esercitazioni precedenti la memorizzazione nella cache, i dati memorizzati nella cache possono diventare non aggiornati. Per ovviare al problema, è possibile usare ora - o oggetti scaduti nella dipendenza basato su SQL cache.

> [!NOTE]
> Un provider di mappe del sito può eseguire l'override di [ `Initialize` metodo](https://msdn.microsoft.com/en-us/library/system.web.sitemapprovider.initialize.aspx). `Initialize`viene richiamato quando il provider della mappa del sito viene innanzitutto creata un'istanza e viene passato a tutti gli attributi personalizzati assegnati al provider in `Web.config` nel `<add>` elemento, ad esempio: `<add name="name" type="type" customAttribute="value" />`. È utile se si desidera consentire a uno sviluppatore di pagina specificare diverse impostazioni correlate al provider della mappa del sito senza dover modificare il codice del provider s. Ad esempio, se è durante la lettura dei prodotti e categoria di dati direttamente dal database anziché tramite l'architettura, d è probabilmente necessario consentire allo sviluppatore della pagina di specificare la stringa di connessione di database tramite `Web.config` invece di usare un livello di codice valore nel codice del provider s. Il provider della mappa del sito personalizzato verrà creato nel passaggio 6 non eseguirne l'override `Initialize` metodo. Per un esempio di utilizzo di `Initialize` (metodo), fare riferimento a [Jeff Prosise](http://www.wintellect.com/Weblogs/CategoryView,category,Jeff%20Prosise.aspx) s [l'archiviazione delle mappe del sito in SQL Server](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) articolo.


## <a name="step-6-creating-the-custom-site-map-provider"></a>Passaggio 6: Creazione di Provider della mappa del sito personalizzato

Per creare un provider di mappe del sito personalizzato che consente di creare la mappa del sito dalle categorie e i prodotti del database Northwind, è necessario creare una classe che estende `StaticSiteMapProvider`. Nel passaggio 1 viene richiesto di aggiungere un `CustomProviders` cartella la `App_Code` cartella - aggiungere una nuova classe in questa cartella denominata `NorthwindSiteMapProvider`. Aggiungere il codice seguente alla classe `NorthwindSiteMapProvider` :


[!code-vb[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample6.vb)]

Consente di iniziare con l'esplorazione di questa classe s s `BuildSiteMap` metodo, che inizia con un [ `lock` istruzione](https://msdn.microsoft.com/en-us/library/c5kehkcz.aspx). Il `lock` istruzione consente un solo thread alla volta immesse, in tal modo l'accesso al relativo codice di serializzazione e impedisce l'esecuzione di istruzioni in una mano reciproca s due thread simultanei.

Il livello di classe `SiteMapNode` variabile `root` viene utilizzato per memorizzare nella cache della struttura della mappa del sito. Quando viene costruita la mappa del sito per la prima volta oppure per la prima volta dopo aver modificati i dati sottostanti, `root` sarà `Nothing` e verrà creata la struttura della mappa del sito. È assegnato il nodo radice mappa s sito `root` durante la costruzione processo in modo che alla successiva esecuzione di questo metodo viene chiamato, `root` non sarà `Nothing`. Di conseguenza, purché `root` non `Nothing` la struttura della mappa del sito verrà restituita al chiamante senza dover ricreare l'oggetto.

Se è radice `Nothing`, la struttura della mappa del sito viene creata dalle informazioni di prodotto e categoria. Mappa del sito viene compilata mediante la creazione di `SiteMapNode` istanze e quindi formano la gerarchia mediante chiamate al `StaticSiteMapProvider` classe s `AddNode` (metodo). `AddNode`esegue la gestione interna, l'archiviazione di diversi `SiteMapNode` le istanze in un `Hashtable`. Prima di iniziare la creazione della gerarchia, viene innanzitutto chiamata la `Clear` metodo, che cancella gli elementi dall'interno `Hashtable`. Successivamente, il `ProductsBLL` classe s `GetProducts` metodo e il valore risultante `ProductsDataTable` vengono archiviati in variabili locali.

La creazione delle mappe s sito inizia creando il nodo radice e assegnarlo alla `root`. L'overload del metodo di [ `SiteMapNode` costruttore s](https://msdn.microsoft.com/en-us/library/system.web.sitemapnode.sitemapnode.aspx) utilizzato qui e in tutto il `BuildSiteMap` viene passato le informazioni seguenti:

- Un riferimento al provider di mappa del sito (`Me`).
- Il `SiteMapNode` s `Key`. Questa richiesta di valore deve essere univoco per ogni `SiteMapNode`.
- Il `SiteMapNode` s `Url`. `Url`è facoltativo, tuttavia, se specificato, ogni `SiteMapNode` s `Url` valore deve essere univoco.
- Il `SiteMapNode` s `Title`, che è necessario.

Il `AddNode(root)` chiamata al metodo aggiunge il `SiteMapNode` `root` alla mappa del sito come radice. Successivamente, ogni `ProductRow` nel `ProductsDataTable` viene enumerata. Se esiste già un `SiteMapNode` per la categoria di prodotto s corrente, fa riferimento. In caso contrario, un nuovo `SiteMapNode` per la categoria viene creata e aggiunto come elemento figlio di `SiteMapNode``root` tramite il `AddNode(categoryNode, root)` chiamata al metodo. Dopo la categoria appropriata `SiteMapNode` nodo è stato trovato o creato, un `SiteMapNode` viene creato per il prodotto corrente e aggiunto come elemento figlio della categoria `SiteMapNode` tramite `AddNode(productNode, categoryNode)`. Si noti che la categoria `SiteMapNode` s `Url` valore della proprietà è `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID=categoryID` mentre il prodotto `SiteMapNode` s `Url` proprietà viene assegnato `~/SiteMapNode/ProductDetails.aspx?ProductID=productID`.

> [!NOTE]
> I prodotti che dispongono di un database `NULL` valore per loro `CategoryID` vengono raggruppate in una categoria `SiteMapNode` cui `Title` proprietà è impostata su None e il cui `Url` è impostata su una stringa vuota. Ho deciso di impostare `Url` su una stringa vuota dopo il `ProductBLL` classe s `GetProductsByCategory(categoryID)` (metodo) non dispone attualmente in grado di restituire solo i prodotti con un `NULL` `CategoryID` valore. Inoltre, per dimostrare come viene eseguito il rendering di controlli di spostamento vorrei un `SiteMapNode` che non dispone di un valore per il relativo `Url` proprietà. Si consiglia di estendere questa esercitazione in modo che nessun `SiteMapNode` s `Url` proprietà punta a `ProductsByCategory.aspx`, ancora vengono visualizzati solo i prodotti con `NULL` `CategoryID` valori.


Al termine della creazione della mappa del sito, un oggetto arbitrario viene aggiunto alla cache di dati utilizzando una dipendenza della cache SQL nel `Categories` e `Products` tabelle tramite un `AggregateCacheDependency` oggetto. Abbiamo esaminate con le dipendenze della cache SQL nell'esercitazione precedente *con dipendenze della Cache SQL*. Il provider della mappa del sito personalizzato, tuttavia, usato un overload della cache dei dati s `Insert` metodo che ve ancora per esplorare. Questo overload accetta come parametro di input finale di un delegato che viene chiamato quando l'oggetto viene rimosso dalla cache. In particolare, viene passato in una nuova [ `CacheItemRemovedCallback` delegato](https://msdn.microsoft.com/en-us/library/system.web.caching.cacheitemremovedcallback.aspx) che punta al `OnSiteMapChanged` metodo definito più avanti nel `NorthwindSiteMapProvider` classe.

> [!NOTE]
> La rappresentazione in memoria della mappa del sito viene memorizzato nella cache tramite la variabile a livello di classe `root`. Perché è presente solo un'istanza della classe del provider di mappa del sito personalizzato e poiché tale istanza viene condiviso tra tutti i thread nell'applicazione web, questa variabile di classe funge da una cache. Il `BuildSiteMap` metodo utilizza anche la cache dei dati, ma solo come mezzo per ricevere una notifica quando i dati del database sottostante il `Categories` o `Products` le tabelle delle modifiche. Si noti che il valore inserito nella cache di dati solo la data e ora correnti. I dati della mappa del sito effettivo sono *non* inserire nella cache dei dati.


Il `BuildSiteMap` completamento del metodo restituendo il nodo radice della mappa del sito.

I metodi rimanenti sono piuttosto semplici. `GetRootNodeCore`è responsabile della restituzione del nodo radice. Poiché `BuildSiteMap` restituisce la radice, `GetRootNodeCore` restituisce semplicemente `BuildSiteMap` s valore restituito. Il `OnSiteMapChanged` metodo imposta `root` al `Nothing` quando viene rimosso l'elemento della cache. Con radice reimpostarlo a `Nothing`, alla successiva `BuildSiteMap` viene richiamato, la struttura della mappa del sito verrà ricostruita. Infine, il `CachedDate` proprietà restituisce il valore di data e ora memorizzato nella cache dei dati, se esiste un valore di questo tipo. Questa proprietà può essere utilizzata dagli sviluppatori di pagine per determinare quando i dati della mappa del sito ultima memorizzate nella cache.

## <a name="step-7-registering-thenorthwindsitemapprovider"></a>Passaggio 7: Registrazione di`NorthwindSiteMapProvider`

Affinché l'applicazione web per l'utilizzo di `NorthwindSiteMapProvider` provider della mappa del sito creato nel passaggio 6, è necessario registrarlo nel `<siteMap>` sezione di `Web.config`. In particolare, aggiungere il markup seguente all'interno di `<system.web>` elemento `Web.config`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample7.xml)]

Questo markup eseguite due operazioni: in primo luogo, che indica l'elemento predefinito `AspNetXmlSiteMapProvider` è il provider della mappa del sito predefinito; secondo, registra il provider della mappa del sito personalizzato creato nel passaggio 6 con il nome descrittivo umane Northwind.

> [!NOTE]
> Per i provider di mappa del sito si trova in s applicazione `App_Code` cartella, il valore della `type` attributo è semplicemente il nome della classe. In alternativa, il provider della mappa del sito personalizzato potrebbe essere stato creato in un progetto libreria di classi separato con l'assembly compilato in modalità di applicazione web s `/Bin` directory. In tal caso, il `type` sarebbe il valore di attributo *Namespace*. *ClassName*, *AssemblyName* .


Dopo aver aggiornato `Web.config`, dedicare alcuni minuti per visualizzare qualsiasi pagina le esercitazioni in un browser. Si noti che l'interfaccia di navigazione a sinistra illustra comunque le sezioni ed esercitazioni definito in `Web.sitemap`. In questo modo è stato lasciato `AspNetXmlSiteMapProvider` come provider predefinito. Per creare un elemento dell'interfaccia utente di spostamento che utilizza il `NorthwindSiteMapProvider`, sarà necessario specificare in modo esplicito che deve essere utilizzato il provider della mappa del sito di Northwind. Vedremo come effettuare questa operazione nel passaggio 8.

## <a name="step-8-displaying-site-map-information-using-the-custom-site-map-provider"></a>Passaggio 8: Visualizzazione di informazioni relative alla mappa del sito utilizzando il Provider di mappa del sito personalizzato

Con il sito personalizzato provider di mappe creato e registrato `Web.config`, è nuovamente pronto per l'aggiunta di controlli di navigazione per il `Default.aspx`, `ProductsByCategory.aspx`, e `ProductDetails.aspx` di pagine nel `SiteMapProvider` cartella. Aprire il `Default.aspx` pagina e trascinare un `SiteMapPath` dalla casella degli strumenti nella finestra di progettazione. Il controllo SiteMapPath si trova nell'area di navigazione della casella degli strumenti.


[![Aggiungere un SiteMapPath Default.aspx](building-a-custom-database-driven-site-map-provider-vb/_static/image19.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image18.gif)

**Figura 16**: aggiungere un SiteMapPath per `Default.aspx` ([fare clic per visualizzare l'immagine ingrandita](building-a-custom-database-driven-site-map-provider-vb/_static/image20.gif))


Visualizza una barra di navigazione, che indica la posizione s della pagina corrente all'interno della mappa del sito. È stato aggiunto un SiteMapPath alla parte superiore della pagina master nuovamente il *pagine Master e esplorazione del sito* esercitazione.

Richiedere qualche istante per visualizzare questa pagina tramite un browser. Il controllo SiteMapPath aggiunto nella figura 16 utilizza il provider di mappa del sito predefinito, i dati da estrarre `Web.sitemap`. Pertanto, la barra di navigazione Mostra Home &gt; personalizzazione mappa del sito, come nell'angolo superiore destro della barra di navigazione.


[![Il percorso utilizza il Provider di mappa del sito predefinito](building-a-custom-database-driven-site-map-provider-vb/_static/image22.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image21.gif)

**Figura 17**: il percorso utilizza il Provider di mappa del sito predefinito ([fare clic per visualizzare l'immagine ingrandita](building-a-custom-database-driven-site-map-provider-vb/_static/image23.gif))


Per utilizzare il provider della mappa del sito personalizzato creato nel passaggio 6 il controllo SiteMapPath aggiunto nella figura 16, impostare il relativo [ `SiteMapProvider` proprietà](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.sitemappath.sitemapprovider.aspx) a Northwind, il nome è assegnato al `NorthwindSiteMapProvider` in `Web.config`. Purtroppo, la finestra di progettazione continua a utilizzare il provider di mappa del sito predefinito, ma se si visita la pagina tramite un browser dopo aver apportato questa modifica della proprietà, si noterà che il percorso ora utilizza il provider della mappa del sito personalizzato.


[![La barra di navigazione Usa ora la NorthwindSiteMapProvider Provider mappa del sito personalizzato](building-a-custom-database-driven-site-map-provider-vb/_static/image25.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image24.gif)

**Figura 18**: il percorso ora utilizza il Provider di mappa del sito personalizzato `NorthwindSiteMapProvider` ([fare clic per visualizzare l'immagine ingrandita](building-a-custom-database-driven-site-map-provider-vb/_static/image26.gif))


Visualizza un'interfaccia utente più funzionale nel `ProductsByCategory.aspx` e `ProductDetails.aspx` pagine. Aggiungere un SiteMapPath queste pagine, l'impostazione di `SiteMapProvider` proprietà sia a Northwind. Da `Default.aspx` fare clic sul collegamento Visualizza i prodotti relativi alle bevande, quindi sul collegamento Visualizza dettagli per Chai tè. Come mostrato nella figura 19, della barra di navigazione include la sezione di mappa del sito corrente (Chai tè) e i relativi predecessori: Beverages e tutte le categorie.


[![La barra di navigazione Usa ora la NorthwindSiteMapProvider Provider mappa del sito personalizzato](building-a-custom-database-driven-site-map-provider-vb/_static/image27.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image21.png)

**Figura 19**: il percorso ora utilizza il Provider di mappa del sito personalizzato `NorthwindSiteMapProvider` ([fare clic per visualizzare l'immagine ingrandita](building-a-custom-database-driven-site-map-provider-vb/_static/image22.png))


Altri elementi dell'interfaccia utente di spostamento è utilizzabile oltre il controllo SiteMapPath, ad esempio i controlli Menu e TreeView. Il `Default.aspx`, `ProductsByCategory.aspx`, e `ProductDetails.aspx` pagine di download per questa esercitazione, ad esempio, includono tutti i controlli Menu (vedere Figura 20). Vedere [s ASP.NET 2.0 esaminare le funzionalità di spostamento sito](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx) e [utilizzando i controlli di spostamento sito](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/navigation/sitenavcontrols.aspx) sezione del [Guide rapide di ASP.NET 2.0](https://quickstarts.asp.net/QuickStartv20/aspnet/) per un approfondimento il controlli di spostamento e il sistema di mappa del sito in ASP.NET 2.0.


[![Il controllo Menu viene elencato ogni e delle categorie prodotti](building-a-custom-database-driven-site-map-provider-vb/_static/image29.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image28.gif)

**Figura 20**: il Menu di controllo Elenca ogni e delle categorie prodotti ([fare clic per visualizzare l'immagine ingrandita](building-a-custom-database-driven-site-map-provider-vb/_static/image30.gif))


Come accennato in precedenza in questa esercitazione, la struttura della mappa del sito è possibile accedere a livello di codice tramite la `SiteMap` classe. Il codice seguente restituisce la radice `SiteMapNode` del provider predefinito:


[!code-vb[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample8.vb)]

Poiché il `AspNetXmlSiteMapProvider` è il provider predefinito per l'applicazione, il codice precedente restituirà il nodo radice definito in `Web.sitemap`. Per fare riferimento a un provider di mappe del sito diverso da quello predefinito, utilizzare il `SiteMap` classe s [ `Providers` proprietà](https://msdn.microsoft.com/en-us/library/system.web.sitemap.providers.aspx) come illustrato di seguito:


[!code-vb[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample9.vb)]

Dove *nome* è il nome del provider della mappa del sito personalizzato (per l'applicazione web, Northwind).

Per accedere a un membro specifico per un provider di mappe del sito, utilizzare `SiteMap.Providers["name"]` per recuperare l'istanza del provider e quindi eseguirne il cast nel tipo appropriato. Ad esempio, per visualizzare il `NorthwindSiteMapProvider` s `CachedDate` proprietà in una pagina ASP.NET, utilizzare il codice seguente:


[!code-vb[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample10.vb)]

> [!NOTE]
> Assicurarsi di testare la funzionalità di dipendenza della cache SQL. Dopo la visita di `Default.aspx`, `ProductsByCategory.aspx`, e `ProductDetails.aspx` pagine, passare a una delle esercitazioni in modalità di modifica, inserimento ed eliminazione di sezione e modificare il nome di una categoria o il prodotto. Tornare a una delle pagine di `SiteMapProvider` cartella. Supponendo che sia trascorso sufficiente tempo per il meccanismo di polling per la modifica al database sottostante, la mappa del sito deve essere aggiornata per mostrare il nome di categoria o un nuovo prodotto.


## <a name="summary"></a>Riepilogo

ASP.NET 2.0 include funzionalità della mappa del sito s un `SiteMap` (classe), un numero di spostamento incorporate Web controlli e un provider di mappe del sito predefinita che prevede che le informazioni della mappa del sito persistenti in un file XML. Per utilizzare le informazioni sulla mappa del sito da un'altra origine, ad esempio da un database, l'architettura dell'applicazione s o un servizio Web remoto, che è necessario creare un provider di mappe personalizzate del sito. Ciò comporta la creazione di una classe derivata, direttamente o indirettamente, dalla `SiteMapProvider` classe.

In questa esercitazione è stato illustrato come creare un provider di mappe personalizzate del sito che la mappa del sito sulla base delle product e category informazioni raccolti dall'architettura di applicazione. Il provider esteso il `StaticSiteMapProvider` classe e la conseguente creazione di un `BuildSiteMap` metodo di recuperata dei dati, costruito gerarchia della mappa del sito e memorizzati nella cache la struttura risultante in una variabile a livello di classe. È stata utilizzata una dipendenza della cache SQL con una funzione di callback per invalidare memorizzato nella cache quando struttura sottostante `Categories` o `Products` i dati vengono modificati.

Buona programmazione!

## <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [L'archiviazione delle mappe del sito in SQL Server](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) e [il Provider di mappa del sito SQL è già stato di attesa per](https://msdn.microsoft.com/msdnmag/issues/06/02/wickedcode/default.aspx)
- [Modello di Provider s un'occhiata a ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)
- [Il Toolkit del Provider](https://msdn.microsoft.com/en-us/asp.net/aa336558.aspx)
- [Analisi di ASP.NET 2.0 s funzionalità di navigazione del sito](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Lead revisori per questa esercitazione sono stati Dave Gardner, Zack Jones, Teresa Murphy e Bernadette Leigh. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Precedente](building-a-custom-database-driven-site-map-provider-cs.md)
