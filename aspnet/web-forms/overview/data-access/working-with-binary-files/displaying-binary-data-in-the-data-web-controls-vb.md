---
uid: web-forms/overview/data-access/working-with-binary-files/displaying-binary-data-in-the-data-web-controls-vb
title: Visualizzazione di dati binari nel Web dei dati controlla (VB) | Microsoft Docs
author: rick-anderson
description: Questa esercitazione esamina le opzioni per presentare i dati binari in una pagina Web, tra cui la visualizzazione di un file di immagine e il provisioning di un collegamento 'Scarica' f...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/27/2007
ms.topic: article
ms.assetid: 9201656a-e1c2-4020-824b-18fb632d2925
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/displaying-binary-data-in-the-data-web-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: a9d298ef328e951f235a6cfcd41b73fafefb0dfb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37373107"
---
<a name="displaying-binary-data-in-the-data-web-controls-vb"></a>Visualizzazione di dati binari nei controlli Web dei dati (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'App di esempio](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_55_VB.exe) o [Scarica il PDF](displaying-binary-data-in-the-data-web-controls-vb/_static/datatutorial55vb1.pdf)

> In questa esercitazione si esamina le opzioni per presentare i dati binari in una pagina Web, tra cui la visualizzazione di un file di immagine e il provisioning di un collegamento 'Scarica' per un file con estensione PDF.


## <a name="introduction"></a>Introduzione

Nell'esercitazione precedente abbiamo esplorato le due tecniche per l'associazione di dati binari con un modello di dati dell'applicazione sottostante s e utilizzato il controllo FileUpload per caricare i file da un browser nel file System s server web. È stato ancora per informazioni su come associare i dati binari caricati con il modello di dati. Vale a dire, dopo che un file caricato e salvato nel file System, un percorso del file deve essere archiviato nel record del database appropriato. Se i dati vengono archiviati direttamente nel database, i dati binari caricati non devono essere salvati nel file System, ma devono essere inseriti nel database.

Prima di esaminare i dati di associazione con il modello di dati, tuttavia, ora s prima di tutto esaminata la fornire i dati binari per l'utente finale. Presentazione dei dati di testo è abbastanza semplice, ma come dati binari da presentare? Dipende, naturalmente, il tipo di dati binari. Per le immagini, è probabile che si voglia visualizzare l'immagine, per i documenti PDF, documenti di Microsoft Word, i file ZIP e altri tipi di dati binari, che fornisce un collegamento di Download è probabilmente più appropriati.

In questa esercitazione che si esaminerà come presentare i dati binari insieme ai relativi dati di testo associato utilizzando i dati come GridView e DetailsView i controlli Web. Nella prossima esercitazione verrà attiviamo prestare attenzione all'associazione di un file caricato con il database.

## <a name="step-1-providingbrochurepathvalues"></a>Passaggio 1: Fornire`BrochurePath`valori

Il `Picture` colonna il `Categories` tabella contiene già dati binari per le diverse immagini di categoria. In particolare, il `Picture` colonna per ogni record contiene il contenuto binario di un'immagine bitmap non importa, bassa qualità e 16 colori. Ogni immagine di categoria è 172 pixel in larghezza e 120 pixel di altezza e consuma circa 11 KB. Altre novità, il contenuto binario nel `Picture` colonna include un byte 78 [OLE](http://en.wikipedia.org/wiki/Object_Linking_and_Embedding) intestazione che deve essere rimosso prima di visualizzare l'immagine. Queste informazioni di intestazione sono presente perché il database Northwind ha le sue radici in Microsoft Access. In Access e i dati binari vengono archiviati utilizzando il tipo di dati oggetto OLE, che consente di tracciare questa intestazione. Per il momento vedremo come rimuovere le intestazioni da queste immagini di bassa qualità per visualizzare l'immagine. In un'esercitazione futura verrà compilata un'interfaccia per l'aggiornamento di una categoria s `Picture` colonna e sostituire queste immagini bitmap che usano OLE intestazioni con immagini JPG equivalente senza le intestazioni OLE non necessari.

Nell'esercitazione precedente abbiamo visto come utilizzare il controllo FileUpload. Pertanto, è possibile procedere e aggiungere i file brochure nel file System s server web. In questo modo, tuttavia, non aggiorna il `BrochurePath` colonna il `Categories` tabella. Nell'esercitazione successiva si vedrà come eseguire questa operazione, ma per ora è necessario specificare manualmente i valori per questa colonna.

In questo download di esercitazione s sono disponibili sette brochure file PDF di `~/Brochures` cartella, uno per ogni categoria eccetto frutti di mare. Intenzionalmente omessa aggiungendo una brochure di frutti di mare per illustrare come gestire scenari in cui non tutti i record sono associati dati binari. Per aggiornare il `Categories` con questi valori di tabella, fare clic su di `Categories` nodo da Esplora Server e scegliere Mostra dati tabella. Quindi, immettere i percorsi virtuali per i file brochure per ogni categoria con una brochure, come illustrato nella figura 1. Poiché non esiste alcun brochure per la categoria di frutti di mare, lasciare relativi `BrochurePath` valore della colonna s come `NULL`.


[![Immettere manualmente i valori per la colonna di BrochurePath tabella s categorie](displaying-binary-data-in-the-data-web-controls-vb/_static/image1.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image1.png)

**Figura 1**: immettere manualmente i valori per il `Categories` tabella s `BrochurePath` colonna ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-binary-data-in-the-data-web-controls-vb/_static/image2.png))


## <a name="step-2-providing-a-download-link-for-the-brochures-in-a-gridview"></a>Passaggio 2: Fornire un collegamento di Download per brochure in GridView

Con il `BrochurePath` i valori specificati per il `Categories` tabella, sono pronti per creare un controllo GridView che elenca ogni categoria insieme a un collegamento per scaricare il brochure categoria s. Nel passaggio 4 amplierò questo GridView per visualizzare anche l'immagine della categoria s.

Per iniziare, trascinare un controllo GridView dalla casella degli strumenti nella finestra di progettazione della `DisplayOrDownloadData.aspx` nella pagina di `BinaryData` cartella. Impostare la s GridView `ID` a `Categories` tramite GridView s smart tag, scegliere da associare a una nuova origine dati. In particolare, associarlo a ObjectDataSource denominato `CategoriesDataSource` che consente di recuperare dati usando il `CategoriesBLL` oggetto s `GetCategories()` (metodo).


[![Figura 10![: il ](displaying-binary-data-in-the-data-web-controls-vb/_static/image2.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image3.png) e  controlli DropDownList includono (nessuna) opzione (fare clic per visualizzare l'immagine con dimensioni normali)](displaying-binary-data-in-the-data-web-controls-vb/_static/image2.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image3.png)

**Figura 2**: creare un nuovo oggetto ObjectDataSource denominato `CategoriesDataSource` ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-binary-data-in-the-data-web-controls-vb/_static/image4.png))


[![Configurare ObjectDataSource per usare la classe CategoriesBLL](displaying-binary-data-in-the-data-web-controls-vb/_static/image3.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image5.png)

**Figura 3**: configurare ObjectDataSource per usare il `CategoriesBLL` classe ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-binary-data-in-the-data-web-controls-vb/_static/image6.png))


[![Recuperare l'elenco di categorie usando il metodo GetCategories()](displaying-binary-data-in-the-data-web-controls-vb/_static/image4.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image7.png)

**Figura 4**: recuperare l'elenco di categorie usando il `GetCategories()` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-binary-data-in-the-data-web-controls-vb/_static/image8.png))


Dopo aver completato la procedura guidata Configura origine dati, Visual Studio aggiungerà automaticamente un BoundField per il `Categories` GridView per il `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`, e `BrochurePath` `DataColumn` s. Proseguire e rimuovere i `NumberOfProducts` BoundField poiché il `GetCategories()` query metodo s recuperare queste informazioni. Rimuovere anche i `CategoryID` BoundField e rinominare il `CategoryName` e `BrochurePath` BoundField `HeaderText` le proprietà per categoria e Brochure, rispettivamente. Dopo aver apportato queste modifiche, il markup dichiarativo s GridView e ObjectDataSource dovrebbe essere simile al seguente:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample1.aspx)]

Visualizzare la pagina tramite un browser (vedere la figura 5). Ognuna delle otto categorie è elencato. Le sette categorie con `BrochurePath` hanno valori di `BrochurePath` zobrazené BoundField il rispettivo valore. Frutti di mare, che ha un `NULL` valore per la relativa `BrochurePath`, consente di visualizzare una cella vuota.


[![Viene elencato ogni categoria s nome, descrizione e valore BrochurePath](displaying-binary-data-in-the-data-web-controls-vb/_static/image5.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image9.png)

**Figura 5**: ogni categoria s, nome, descrizione, e `BrochurePath` valore è elencato ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-binary-data-in-the-data-web-controls-vb/_static/image10.png))


Anziché visualizzare il testo del `BrochurePath` colonna, è necessario creare un collegamento a brochure. A tale scopo, rimuovere il `BrochurePath` BoundField e sostituirlo con un HyperLinkField. Impostare la nuova s HyperLinkField `HeaderText` proprietà Brochure, relativi `Text` proprietà alla visualizzazione Brochure e i relativi `DataNavigateUrlFields` proprietà `BrochurePath`.


![Aggiungere un HyperLinkField per BrochurePath](displaying-binary-data-in-the-data-web-controls-vb/_static/image6.gif)

**Figura 6**: aggiungere un HyperLinkField per `BrochurePath`


Verrà aggiunta una colonna di collegamenti a GridView, come illustrato nella figura 7. Facendo clic su un collegamento Brochure di visualizzazione verrà visualizzare il file PDF direttamente nel browser o richiedere all'utente di scaricare il file, a seconda se è installato un file PDF e le impostazioni del browser s.


[![Una categoria s Brochure può essere visualizzati facendo clic sul collegamento Visualizza Brochure](displaying-binary-data-in-the-data-web-controls-vb/_static/image7.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image11.png)

**Figura 7**: una categoria s Brochure possono essere visualizzati facendo clic sul collegamento Visualizza Brochure ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-binary-data-in-the-data-web-controls-vb/_static/image12.png))


[![Viene visualizzata la categoria s Brochure PDF](displaying-binary-data-in-the-data-web-controls-vb/_static/image8.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image13.png)

**Figura 8**: la categoria s Brochure PDF viene visualizzato ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-binary-data-in-the-data-web-controls-vb/_static/image14.png))


## <a name="hiding-the-view-brochure-text-for-categories-without-a-brochure"></a>Nascondere il testo Brochure di visualizzazione per categorie senza una Brochure

Come illustrato nella figura 7, il `BrochurePath` HyperLinkField Visualizza relativa `Text` il valore della proprietà (vista Brochure) per tutti i record, indipendentemente dal fatto che s non esiste`NULL` value per `BrochurePath`. Naturalmente, se `BrochurePath` è `NULL`, quindi il collegamento viene visualizzato come solo testo, come avviene con la categoria di frutti di mare (vedere la figura 7). Anziché visualizzare il testo Brochure di visualizzazione, potrebbe essere utile specificare tali categorie senza un `BrochurePath` valore visualizzano il testo alternativo, ad esempio nessun Brochure disponibile.

Per fornire questo comportamento, è necessario usare un TemplateField il cui contenuto viene generato tramite una chiamata a un metodo di pagina che genera output appropriato in base il `BrochurePath` valore. È prima di tutto esaminato questa tecnica di formattazione nel [uso di TemplateFields nel controllo GridView](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) esercitazione.

Attivare il HyperLinkField in un TemplateField selezionando il `BrochurePath` HyperLinkField e quindi facendo clic su Converti questo campo in un TemplateField collegamento nella finestra di dialogo Modifica colonne.


![Convertire il HyperLinkField in un TemplateField](displaying-binary-data-in-the-data-web-controls-vb/_static/image9.gif)

**Figura 9**: convertire il HyperLinkField in un TemplateField


Verrà creato un TemplateField con un `ItemTemplate` che contiene un collegamento ipertestuale Web controllo la cui `NavigateUrl` proprietà è associata la `BrochurePath` valore. Sostituire questo markup con una chiamata al metodo `GenerateBrochureLink`, passando il valore di `BrochurePath`:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample2.aspx)]

A questo punto, creare un `Protected` metodo in ASP.NET pagina classe code-behind s denominato `GenerateBrochureLink` che restituisce un `String` e accetta un `Object` come parametro di input.


[!code-vb[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample3.vb)]

Questo metodo determina se passato `Object` valore è un database `NULL` e, in caso affermativo, restituisce un messaggio che indica che la categoria non dispone di una brochure. In caso contrario, se è presente un `BrochurePath` valore, viene visualizzato in un collegamento ipertestuale. Si noti che se il `BrochurePath` valore è presentarli s passato il [ `ResolveUrl(url)` metodo](https://msdn.microsoft.com/library/system.web.ui.control.resolveurl.aspx). Questo metodo risolve passato *url*, sostituendo il `~` carattere con il percorso virtuale appropriato. Ad esempio, se l'applicazione ha origine nel `/Tutorial55`, `ResolveUrl("~/Brochures/Meats.pdf")` restituirà `/Tutorial55/Brochures/Meat.pdf`.

Figura 10 Mostra la pagina dopo aver applicate queste modifiche. Si noti che la categoria di frutti di mare s `BrochurePath` campo verrà visualizzato il testo No Brochure disponibili.


[![Testo Nessun Brochure disponibile viene visualizzato per le categorie senza una Brochure](displaying-binary-data-in-the-data-web-controls-vb/_static/image10.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image15.png)

**Figura 10**: viene visualizzato il testo No Brochure disponibile per quelle categorie senza una Brochure ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-binary-data-in-the-data-web-controls-vb/_static/image16.png))


## <a name="step-3-adding-a-web-page-to-display-a-category-s-picture"></a>Passaggio 3: Aggiunta di una pagina Web per visualizzare un'immagine di categoria s

Quando un utente visita una pagina ASP.NET, l'utente riceve la pagina s HTML di ASP.NET. Il codice HTML ricevuto solo il testo e non contenere dati binari. Qualsiasi dato binario aggiuntive, ad esempio immagini, file audio, applicazioni di Macromedia Flash, embedded video Windows Media Player e così via, esistono come risorse separate sul server web. Il codice HTML contiene riferimenti a questi file, ma non include il contenuto effettivo dei file.

Ad esempio, in formato HTML il `<img>` elemento viene usato per fare riferimento a un'immagine, con la `src` attributo che punta al file di immagine in questo modo:


[!code-html[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample4.html)]

Quando un browser riceve questo codice HTML, esegue un'altra richiesta al server web per recuperare il contenuto binario del file di immagine, che viene quindi visualizzato nel browser. Lo stesso concetto si applica a qualsiasi dato binario. Nel passaggio 2, brochure non è stata inviata verso il basso nel browser come parte del markup HTML s della pagina. Piuttosto, il codice HTML forniti i collegamenti ipertestuali che, quando si fa clic, ha causato il browser richiedere il documento PDF direttamente.

Per visualizzare o per consentire agli utenti di scaricare i dati binari che si trovano all'interno del database, è necessario creare una pagina web separata che restituisce i dati. Per la nostra applicazione, tale posizione s un solo i dati binari campo archiviato direttamente nell'immagine database s categoria. Dobbiamo quindi una pagina che, quando viene chiamato, restituisce i dati dell'immagine per una determinata categoria.

Aggiungere una nuova pagina ASP.NET per la `BinaryData` cartella denominata `DisplayCategoryPicture.aspx`. In tal caso, lasciare deselezionata la casella di controllo Seleziona pagina master. Questa pagina si aspetta un `CategoryID` valore nella stringa di query e restituisce i dati binari di tale categoria s `Picture` colonna. Poiché questa pagina restituisce dati binari e nient'altro, non è necessario alcun markup nella sezione HTML. Pertanto, fare clic sulla scheda origine nell'angolo inferiore sinistro e rimuovere tutti i markup della pagina s, ad eccezione del `<%@ Page %>` direttiva. Vale a dire, `DisplayCategoryPicture.aspx` markup dichiarativo s deve essere costituito da una singola riga:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample5.aspx)]

Se viene visualizzato il `MasterPageFile` attributo la `<%@ Page %>` direttiva, rimuoverlo.

Nella classe code-behind della pagina s, aggiungere il codice seguente per il `Page_Load` gestore dell'evento:


[!code-vb[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample6.vb)]

Inizia questo codice leggendo la `CategoryID` valore di stringa di query in una variabile denominata `categoryID`. Successivamente, i dati dell'immagine vengono recuperati tramite una chiamata per il `CategoriesBLL` classe s `GetCategoryWithBinaryDataByCategoryID(categoryID)` (metodo). Questi dati vengono restituiti al client tramite il `Response.BinaryWrite(data)` metodo, ma prima che questo metodo viene chiamato, il `Picture` intestazione di colonna valore s OLE deve essere rimossi. Questa operazione viene eseguita tramite la creazione di un `Byte` matrice denominata `strippedImageData` che conterranno esattamente 78 caratteri minore di quali sono le novità di `Picture` colonna. Il [ `Array.Copy` metodo](https://msdn.microsoft.com/library/z50k9bft.aspx) viene usata per copiare i dati dal `category.Picture` a partire dalla posizione 78 su a `strippedImageData`.

Il `Response.ContentType` proprietà consente di specificare il [tipo MIME](http://en.wikipedia.org/wiki/MIME) del contenuto restituito in modo che il browser sa come eseguirne il rendering. Poiché il `Categories` tabella s `Picture` la colonna è un'immagine bitmap, viene usata il tipo MIME di bitmap qui (image/bmp). Se si omette il tipo MIME, la maggior parte dei browser continuerà a essere visualizzato l'immagine correttamente perché si può dedurre il tipo in base al contenuto dei dati binari s file immagine. Tuttavia, è opportuno includere MIME s digitare quando possibile. Vedere le [sito Web Internet Assigned Numbers Authority](http://www.iana.org/) per un elenco completo dei [tipi di supporto MIME](http://www.iana.org/assignments/media-types/).

Con questa pagina creata, un'immagine di particolare categoria s può essere visualizzata visitando `DisplayCategoryPicture.aspx?CategoryID=categoryID`. Figura 11 mostra l'immagine di categoria s bibite, che può essere visualizzato da `DisplayCategoryPicture.aspx?CategoryID=1`.


[![Le s categoria Beverages che visualizzazione immagine](displaying-binary-data-in-the-data-web-controls-vb/_static/image11.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image17.png)

**Figura 11**: la categoria di bevande s immagine viene visualizzata ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-binary-data-in-the-data-web-controls-vb/_static/image18.png))


Se, quando si visita `DisplayCategoryPicture.aspx?CategoryID=categoryID`, si verifica un'eccezione che legge Impossibile per il cast dell'oggetto di tipo 'System. DBNull' nel tipo 'System. Byte []', esistono due aspetti che potrebbero essere la causa. Prima di tutto, il `Categories` tabella s `Picture` colonna Consenti `NULL` valori. Il `DisplayCategoryPicture.aspx` , tuttavia, si presume che è non`NULL` valore attuale. Il `Picture` proprietà del `CategoriesDataTable` non è possibile accedervi direttamente se ha un `NULL` valore. Se si desidera consentire `NULL` i valori per il `Picture` colonna d da includere la condizione seguente:


[!code-vb[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample7.vb)]

Il codice sopra riportato si presuppone che in questo caso s alcuni immagine file denominato `NoPictureAvailable.gif` nella `Images` cartella in cui si desidera visualizzare per queste categorie senza un'immagine.

Questa eccezione potrebbe essere causata anche se il `CategoriesTableAdapter` s `GetCategoryWithBinaryDataByCategoryID` metodo s `SELECT` istruzione ha ripristinato l'elenco di colonne della query principale s, ciò può verificarsi se si Usa istruzioni SQL ad-hoc ed è già eseguire di nuovo la procedura guidata per i TableAdapter query principale. Verificare che `GetCategoryWithBinaryDataByCategoryID` metodo s `SELECT` istruzione include ancora il `Picture` colonna.

> [!NOTE]
> Ogni volta che il `DisplayCategoryPicture.aspx` viene visitato, accedere al database e vengono restituiti i dati di immagine specificato categoria s. Se t categoria s immagine non abbia modificato dopo l'utente ha ultima visualizzazione, tuttavia, questo è inutile. Fortunatamente, HTTP consente *condizionale Ottiene*. Con una richiesta GET condizionale, il client effettua la richiesta HTTP invia lungo un [ `If-Modified-Since` intestazione HTTP](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html) che fornisce la data e ora dell'ultima recuperato questa risorsa dal client al server web. Se il contenuto non è stato modificato poiché questa data non specificato, il server web può rispondere con un [codice di stato non modificato (304)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) e rinunciare reinviato al contenuto della risorsa richiesta s. In breve, questa tecnica evita di dover inviare nuovamente il contenuto di una risorsa se non è stato modificato dall'ultimo accesso, il client di server web.


Per implementare questo comportamento, tuttavia, è necessario aggiungere un `PictureLastModified` colonna per il `Categories` tabella per l'acquisizione quando il `Picture` ultimo aggiornamento della colonna, nonché il codice per verificare la presenza il `If-Modified-Since` intestazione. Per altre informazioni sul `If-Modified-Since` intestazione e il flusso di lavoro condizionale di GET, vedere [HTTP GET condizionale pirati informatici RSS](http://fishbowl.pastiche.org/2002/10/21/http_conditional_get_for_rss_hackers) e [A una maggiore Look at eseguire richieste HTTP in una pagina ASP.NET](http://aspnet.4guysfromrolla.com/articles/122204-1.aspx).

## <a name="step-4-displaying-the-category-pictures-in-a-gridview"></a>Passaggio 4: Visualizzare le immagini di categoria in un oggetto GridView

Ora che abbiamo una pagina web per visualizzare un'immagine di particolare categoria s, è possibile visualizzare tramite il [controllo Web Image](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/standard/image.aspx) o un elemento HTML `<img>` che punta all'elemento `DisplayCategoryPicture.aspx?CategoryID=categoryID`. Immagini di cui URL è determinato dai dati di database possono essere visualizzate nel controllo GridView o DetailsView usando ImageField. Contiene ImageField `DataImageUrlField` e `DataImageUrlFormatString` le proprietà che funzionano come s HyperLinkField `DataNavigateUrlFields` e `DataNavigateUrlFormatString` proprietà.

Let s potenziare il `Categories` GridView in `DisplayOrDownloadData.aspx` aggiungendo un ImageField per mostrare ogni immagine categoria s. È sufficiente aggiungere ImageField e impostare relativi `DataImageUrlField` e `DataImageUrlFormatString` delle proprietà per `CategoryID` e `DisplayCategoryPicture.aspx?CategoryID={0}`, rispettivamente. Si creerà una colonna GridView che esegue il rendering di un `<img>` elemento la cui `src` i riferimenti dell'attributo `DisplayCategoryPicture.aspx?CategoryID={0}`, dove {0} viene sostituito con la riga GridView s `CategoryID` valore.


![Aggiungere un ImageField a GridView](displaying-binary-data-in-the-data-web-controls-vb/_static/image12.gif)

**Figura 12**: aggiungere un ImageField a GridView


Dopo l'aggiunta di ImageField, la sintassi dichiarativa per il controllo GridView s dovrebbe essere come soothe seguenti:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample8.aspx)]

Richiedere qualche istante per visualizzare questa pagina tramite un browser. Si noti come ogni record include ora un'immagine per la categoria.


[![La categoria s immagine è visualizzata per ogni riga](displaying-binary-data-in-the-data-web-controls-vb/_static/image13.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image19.png)

**Figura 13**: s categoria l'immagine viene visualizzata per ogni riga ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-binary-data-in-the-data-web-controls-vb/_static/image20.png))


## <a name="summary"></a>Riepilogo

In questa esercitazione viene esaminato il modo di presentare i dati binari. Come vengono presentati i dati varia a seconda del tipo di dati. Per i file PDF brochure, abbiamo offerto all'utente una Brochure di visualizzazione collegamento che, quando si fa clic, ha richiesto l'utente direttamente nel file PDF. Per l'immagine di s categoria, viene innanzitutto creata una pagina per recuperare e restituire i dati binari dal database e consente quindi di visualizzare ogni immagine s categoria in un controllo GridView tale pagina.

Ora che è stato preso in esame come visualizzare i dati binari, sono pronti per esaminare come eseguire l'inserimento, aggiornamento ed eliminazione sul database con i dati binari. Nell'esercitazione successiva esamineremo come associare un file caricato con record del database corrispondente. Nell'esercitazione in seguito, si vedrà come aggiornare i dati binari esistenti, nonché come eliminare i dati binari relativi record associato viene rimosso.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. I revisori per questa esercitazione sono state Teresa Murphy e Dave Gardner. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](uploading-files-vb.md)
> [Successivo](including-a-file-upload-option-when-adding-a-new-record-vb.md)
