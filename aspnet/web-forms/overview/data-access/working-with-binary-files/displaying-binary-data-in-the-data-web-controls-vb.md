---
uid: web-forms/overview/data-access/working-with-binary-files/displaying-binary-data-in-the-data-web-controls-vb
title: Visualizzazione di dati binari nel Web dati controlla (VB) | Documenti Microsoft
author: rick-anderson
description: In questa esercitazione verranno esaminate le opzioni per presentare i dati binari in una pagina Web, tra cui la visualizzazione di un file di immagine e il provisioning di un collegamento 'Scarica' f...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/27/2007
ms.topic: article
ms.assetid: 9201656a-e1c2-4020-824b-18fb632d2925
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/displaying-binary-data-in-the-data-web-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 006a4d014b610f3079d7f25e9420f687447a1b26
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="displaying-binary-data-in-the-data-web-controls-vb"></a>Visualizzazione di dati binari nei controlli Web dati (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare App di esempio](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_55_VB.exe) o [Scarica il PDF](displaying-binary-data-in-the-data-web-controls-vb/_static/datatutorial55vb1.pdf)

> In questa esercitazione verranno esaminate le opzioni per presentare i dati binari in una pagina Web, tra cui la visualizzazione di un file di immagine e il provisioning di un collegamento 'Scarica' per un file PDF.


## <a name="introduction"></a>Introduzione

Nell'esercitazione precedente si sono stati illustrati due tecniche per l'associazione di dati binari con un modello di dati dell'applicazione sottostante s e utilizza il controllo FileUpload per caricare i file da un browser per il file system s del server web. Si sposta ancora per informazioni su come associare i dati binari caricati il modello di dati. Dopo aver caricato un file e salvato nel file System, un percorso del file debba essere archiviato nel record del database appropriato. Se i dati sono stati archiviati direttamente nel database, i dati binari caricati non devono essere salvati nel file System, ma devono essere inseriti nel database.

Prima di esaminare i dati di associazione con il modello di dati, tuttavia, consentono s innanzitutto analizzato in che modo fornire i dati binari per l'utente finale. La presentazione dei dati di testo è abbastanza semplice, ma come dati binari verranno visualizzati? Dipende, naturalmente, il tipo di dati binari. Per le immagini, è probabile che desidera visualizzare l'immagine, per i file PDF, documenti di Microsoft Word, i file ZIP e altri tipi di dati binari, fornendo un collegamento di Download è probabilmente più appropriati.

In questa esercitazione che verranno esaminati come presentare i dati binari insieme ai relativi dati di testo associato utilizzando i dati Web controlli quali GridView e DetailsView. Nella prossima esercitazione si verranno prese in esame l'associazione di un file caricato con il database.

## <a name="step-1-providingbrochurepathvalues"></a>Passaggio 1: Fornendo`BrochurePath`valori

Il `Picture` colonna il `Categories` tabella contiene già dati binari per le diverse immagini di categoria. In particolare, il `Picture` colonna per ogni record contiene il contenuto binario di un'immagine bitmap muove, bassa qualità a 16 colori. Ogni immagine di categoria è 172 pixel in larghezza e 120 pixel di altezza e utilizza circa 11 KB. Altre novità, il contenuto binario di `Picture` colonna include un 78 byte [OLE](http://en.wikipedia.org/wiki/Object_Linking_and_Embedding) intestazione che deve essere rimossi prima di visualizzare l'immagine. Queste informazioni di intestazione sono presente, perché il database Northwind radici in Microsoft Access. In Access, i dati binari vengono archiviati utilizzando il tipo di dati oggetto OLE, che puntine su questa intestazione. Per il momento, vedremo come rimuovere le intestazioni da queste immagini di bassa qualità per visualizzare l'immagine. In una futura esercitazione creeremo un'interfaccia per l'aggiornamento di una categoria s `Picture` colonna e sostituire le immagini bitmap che utilizzano intestazioni OLE equivalente immagini JPG senza le intestazioni OLE non necessari.

Nell'esercitazione precedente è stato illustrato come utilizzare il controllo FileUpload. Pertanto, è possibile procedere e aggiungere i file brochure nel file System di web server s. In questo modo, tuttavia, non aggiorna il `BrochurePath` colonna il `Categories` tabella. Nella prossima esercitazione si vedrà come effettuare questa operazione, ma per ora è necessario specificare manualmente i valori per questa colonna.

In questo download esercitazione s sono disponibili sette brochure file PDF di `~/Brochures` cartella, uno per ogni categoria, ad eccezione di frutti di mare. Omesso di proposito aggiunta brochure frutti di mare per illustrare in che modo gestire gli scenari in cui non tutti i record sono associati dati binari. Per aggiornare il `Categories` con questi valori di tabella, fare clic su di `Categories` nodo da Esplora Server e scegliere Mostra dati tabella. Quindi, immettere i percorsi virtuali per i file brochure per ogni categoria che ha una brochure, come illustrato nella figura 1. Poiché non è presente alcun brochure per la categoria di frutti di mare, lasciare il `BrochurePath` valore della colonna s come `NULL`.


[![Immettere manualmente i valori per la colonna BrochurePath della tabella s categorie](displaying-binary-data-in-the-data-web-controls-vb/_static/image1.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image1.png)

**Figura 1**: immettere manualmente i valori per il `Categories` tabella s `BrochurePath` colonna ([fare clic per visualizzare l'immagine ingrandita](displaying-binary-data-in-the-data-web-controls-vb/_static/image2.png))


## <a name="step-2-providing-a-download-link-for-the-brochures-in-a-gridview"></a>Passaggio 2: Fornire un collegamento di Download per brochure in un controllo GridView.

Con il `BrochurePath` valori forniti per il `Categories` tabella, è nuovamente pronto per creare un controllo GridView che elenca ogni categoria insieme a un collegamento per scaricare la brochure categoria s. Passaggio 4 sarà estende questo GridView per visualizzare anche l'immagine della categoria s.

Per iniziare, trascinare un controllo GridView dalla casella degli strumenti nella finestra di progettazione della `DisplayOrDownloadData.aspx` nella pagina di `BinaryData` cartella. Impostare i dispositivi di GridView `ID` a `Categories` tramite GridView s smart tag, scegliere di associarlo a una nuova origine dati. In particolare, associarlo a ObjectDataSource denominato `CategoriesDataSource` che consente di recuperare dati usando il `CategoriesBLL` oggetto s `GetCategories()` metodo.


[![Creare un nuovo oggetto ObjectDataSource denominato CategoriesDataSource](displaying-binary-data-in-the-data-web-controls-vb/_static/image2.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image3.png)

**Figura 2**: creare un nuovo denominato ObjectDataSource `CategoriesDataSource` ([fare clic per visualizzare l'immagine ingrandita](displaying-binary-data-in-the-data-web-controls-vb/_static/image4.png))


[![Configurare ObjectDataSource per utilizzare la classe CategoriesBLL](displaying-binary-data-in-the-data-web-controls-vb/_static/image3.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image5.png)

**Figura 3**: configurare ObjectDataSource per usare il `CategoriesBLL` classe ([fare clic per visualizzare l'immagine ingrandita](displaying-binary-data-in-the-data-web-controls-vb/_static/image6.png))


[![Recuperare l'elenco delle categorie utilizzando il metodo GetCategories()](displaying-binary-data-in-the-data-web-controls-vb/_static/image4.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image7.png)

**Figura 4**: recuperare l'elenco di categorie usando il `GetCategories()` metodo ([fare clic per visualizzare l'immagine ingrandita](displaying-binary-data-in-the-data-web-controls-vb/_static/image8.png))


Dopo aver completato la configurazione guidata origine dati, Visual Studio aggiungerà automaticamente un BoundField per il `Categories` GridView per il `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`, e `BrochurePath` `DataColumn` s. Vado avanti e rimuovere il `NumberOfProducts` BoundField dopo il `GetCategories()` query metodo s non recuperare queste informazioni. Rimuovere anche il `CategoryID` BoundField e rinominare il `CategoryName` e `BrochurePath` BoundField `HeaderText` le proprietà per categoria e Brochure, rispettivamente. Dopo aver apportato queste modifiche, il markup dichiarativo s GridView e ObjectDataSource dovrebbe essere simile al seguente:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample1.aspx)]

Visualizzare la pagina tramite un browser (vedere Figura 5). Viene elencato ogni otto categorie. Le sette categorie con `BrochurePath` valori hanno la `BrochurePath` valore visualizzato con il rispettivo BoundField. Frutti di mare, che ha un `NULL` valore per il relativo `BrochurePath`, consente di visualizzare una cella vuota.


[![Viene elencato ogni categoria s nome, descrizione e il valore BrochurePath](displaying-binary-data-in-the-data-web-controls-vb/_static/image5.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image9.png)

**Figura 5**: ogni categoria s, nome, descrizione, e `BrochurePath` valore viene elencato ([fare clic per visualizzare l'immagine ingrandita](displaying-binary-data-in-the-data-web-controls-vb/_static/image10.png))


Anziché visualizzare il testo del `BrochurePath` colonna, di voler creare un collegamento a brochure. A tale scopo, rimuovere il `BrochurePath` BoundField e sostituirlo con un HyperLinkField. Impostare il nuovo s HyperLinkField `HeaderText` proprietà Brochure, il relativo `Text` proprietà alla visualizzazione Brochure e il relativo `DataNavigateUrlFields` proprietà `BrochurePath`.


![Aggiungere un HyperLinkField per BrochurePath](displaying-binary-data-in-the-data-web-controls-vb/_static/image6.gif)

**Figura 6**: aggiungere un HyperLinkField per `BrochurePath`


Verrà aggiunta una colonna di collegamenti a GridView, come illustrato nella figura 7. Fare clic su un collegamento Visualizza Brochure saranno visualizzare il formato PDF direttamente nel browser o richiedere all'utente di scaricare il file, a seconda che sia installato un visualizzatore PDF e le impostazioni del browser s.


[![Una categoria s Brochure può essere visualizzati facendo clic sul collegamento Visualizza Brochure](displaying-binary-data-in-the-data-web-controls-vb/_static/image7.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image11.png)

**Figura 7**: una categoria s Brochure possono essere visualizzati facendo clic sul collegamento Visualizza Brochure ([fare clic per visualizzare l'immagine ingrandita](displaying-binary-data-in-the-data-web-controls-vb/_static/image12.png))


[![Viene visualizzata la categoria s Brochure PDF](displaying-binary-data-in-the-data-web-controls-vb/_static/image8.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image13.png)

**Figura 8**: la categoria s Brochure PDF viene visualizzato ([fare clic per visualizzare l'immagine ingrandita](displaying-binary-data-in-the-data-web-controls-vb/_static/image14.png))


## <a name="hiding-the-view-brochure-text-for-categories-without-a-brochure"></a>Nascondere il testo della Brochure visualizzazione per categorie senza una Brochure

Come illustrato nella figura 7, il `BrochurePath` HyperLinkField consente di visualizzare il relativo `Text` valore della proprietà (vista Brochure) per tutti i record, indipendentemente dal fatto che s non esiste`NULL` valore per `BrochurePath`. Naturalmente, se `BrochurePath` è `NULL`, quindi il collegamento viene visualizzato come solo testo, come accade con la categoria di frutti di mare (vedere la figura 7). Anziché visualizzare il testo di visualizzazione Brochure, potrebbe essere utile disporre di queste categorie senza un `BrochurePath` valore visualizzano il testo alternativo, ad esempio Brochure non disponibile.

Per garantire questo comportamento, è necessario utilizzare un TemplateField il cui contenuto viene generato tramite una chiamata a un metodo di pagina che genera l'output appropriato in base il `BrochurePath` valore. È prima di tutto esplorare questa formattazione tecnica nuovamente il [TemplateFields utilizzo nel controllo GridView](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) esercitazione.

Attivare il HyperLinkField in un TemplateField selezionando il `BrochurePath` HyperLinkField e facendo clic su Converti il campo in un TemplateField collegamento nella finestra di dialogo Modifica colonne.


![Convertire il HyperLinkField in un TemplateField](displaying-binary-data-in-the-data-web-controls-vb/_static/image9.gif)

**Figura 9**: convertire il HyperLinkField in un TemplateField


Verrà creato un TemplateField con un `ItemTemplate` che contiene un collegamento ipertestuale controllo cui `NavigateUrl` proprietà è associata la `BrochurePath` valore. Sostituire questo markup con una chiamata al metodo `GenerateBrochureLink`, passando il valore di `BrochurePath`:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample2.aspx)]

Creare quindi un `Protected` metodo in ASP.NET pagina classe code-behind s denominato `GenerateBrochureLink` che restituisce un `String` e accetta un `Object` come parametro di input.


[!code-vb[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample3.vb)]

Questo metodo determina se passato `Object` valore è un database `NULL` e, in caso affermativo, restituisce un messaggio che indica che la categoria non dispone di una brochure. In caso contrario, se è presente un `BrochurePath` valore, viene visualizzato in un collegamento ipertestuale. Si noti che se il `BrochurePath` valore presentarli s passato il [ `ResolveUrl(url)` metodo](https://msdn.microsoft.com/library/system.web.ui.control.resolveurl.aspx). Questo metodo risolve passato *url*, sostituendo il `~` carattere con il percorso virtuale appropriato. Ad esempio, se l'applicazione è sempre la `/Tutorial55`, `ResolveUrl("~/Brochures/Meats.pdf")` restituirà `/Tutorial55/Brochures/Meat.pdf`.

Figura 10 è illustrata la pagina dopo l'applicazione di queste modifiche. Si noti che la categoria di frutti di mare s `BrochurePath` campo verrà visualizzato il testo Brochure non disponibile.


[![Testo Nessun Brochure disponibile viene visualizzato per quelle categorie senza un Brochure](displaying-binary-data-in-the-data-web-controls-vb/_static/image10.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image15.png)

**Figura 10**: viene visualizzato il testo No Brochure disponibile per quelle categorie senza un Brochure ([fare clic per visualizzare l'immagine ingrandita](displaying-binary-data-in-the-data-web-controls-vb/_static/image16.png))


## <a name="step-3-adding-a-web-page-to-display-a-category-s-picture"></a>Passaggio 3: Aggiunta di una pagina Web per visualizzare un'immagine di categoria s

Quando un utente visita una pagina ASP.NET, l'utente riceve la pagina s HTML di ASP.NET. HTML ricevuto solo il testo e non contiene i dati binari. Dati binari aggiuntivi, come immagini, file audio, applicazioni Macromedia Flash, Windows Media Player video incorporati e così via, esistono come risorse separate sul server web. Il codice HTML contiene riferimenti a questi file, ma non include il contenuto effettivo dei file.

Ad esempio, in formato HTML il `<img>` elemento viene usato per fare riferimento a un'immagine, con la `src` attributo che punta al file di immagine come illustrato di seguito:


[!code-html[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample4.html)]

Quando un browser riceve questo codice HTML, effettua un'altra richiesta al server web per recuperare il contenuto binario del file di immagine, che viene quindi visualizzato nel browser. Lo stesso concetto si applica a tutti i dati binari. Nel passaggio 2, brochure non è stata inviata verso il basso nel browser come parte del markup HTML della pagina s. Invece, il codice HTML forniti collegamenti ipertestuali che, quando si fa clic, ha causato il browser richiedere il documento PDF direttamente.

Per visualizzare o per consentire agli utenti di scaricare i dati binari che si trovano all'interno del database, è necessario creare una pagina web separata che restituisce i dati. Per l'applicazione, s esiste un solo dati binari campo archiviato direttamente nell'immagine database s la categoria. Pertanto, è necessaria una pagina che, quando viene chiamato, restituisce i dati dell'immagine per una categoria specifica.

Aggiungere una nuova pagina ASP.NET per il `BinaryData` cartella denominata `DisplayCategoryPicture.aspx`. In tal caso, lasciare deselezionata la casella di controllo Seleziona pagina master. Questa pagina è previsto un `CategoryID` valore nella stringa di query e restituisce i dati binari di tale categoria s `Picture` colonna. Poiché questa pagina restituisce dati binari e nient'altro, non è necessario il codice nella sezione HTML. Pertanto, fare clic sulla scheda origine nell'angolo inferiore sinistro e rimozione di tutti i markup della pagina s tranne che per il `<%@ Page %>` direttiva. Vale a dire `DisplayCategoryPicture.aspx` markup dichiarativo s deve essere costituito da una singola riga:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample5.aspx)]

Se viene visualizzato il `MasterPageFile` attributo la `<%@ Page %>` direttiva, rimuoverlo.

Nella classe code-behind di pagine, aggiungere il codice seguente per il `Page_Load` gestore eventi:


[!code-vb[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample6.vb)]

Questo codice avvia leggendo il `CategoryID` il valore di stringa di query in una variabile denominata `categoryID`. Successivamente, i dati dell'immagine viene recuperati mediante una chiamata al `CategoriesBLL` classe s `GetCategoryWithBinaryDataByCategoryID(categoryID)` metodo. Questi dati vengono restituiti al client utilizzando il `Response.BinaryWrite(data)` (metodo), ma prima che questo metodo viene chiamato, il `Picture` intestazione di colonna valore s OLE deve essere rimossi. Questa operazione viene eseguita creando un `Byte` matrice denominata `strippedImageData` che conterrà esattamente 78 caratteri rispetto a quello nel `Picture` colonna. Il [ `Array.Copy` metodo](https://msdn.microsoft.com/library/z50k9bft.aspx) viene utilizzato per copiare i dati da `category.Picture` a partire dalla posizione 78 su a `strippedImageData`.

Il `Response.ContentType` proprietà specifica il [tipo MIME](http://en.wikipedia.org/wiki/MIME) del contenuto viene restituito in modo che il browser sia in grado di eseguirne il rendering. Poiché il `Categories` tabella s `Picture` la colonna è un'immagine bitmap, viene utilizzata il tipo MIME di bitmap qui image/bmp (). Se si omette il tipo MIME, la maggior parte dei browser continuerà a visualizzare l'immagine correttamente perché sono in grado di dedurre il tipo in base al contenuto dei dati binari s file dell'immagine. Tuttavia, è consigliabile includere MIME s digitare quando possibile. Vedere il [sito Web di Internet Assigned Numbers Authority s](http://www.iana.org/) per un elenco completo dei [tipi di supporto MIME](http://www.iana.org/assignments/media-types/).

Questa pagina creata, un'immagine di categoria s può essere visualizzata visitando `DisplayCategoryPicture.aspx?CategoryID=categoryID`. Figura 11 mostra l'immagine di categoria s bevande, che può essere visualizzato da `DisplayCategoryPicture.aspx?CategoryID=1`.


[![S categoria Beverages che immagine viene visualizzata](displaying-binary-data-in-the-data-web-controls-vb/_static/image11.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image17.png)

**Figura 11**: la categoria Beverages s immagine viene visualizzata ([fare clic per visualizzare l'immagine ingrandita](displaying-binary-data-in-the-data-web-controls-vb/_static/image18.png))


Se, durante la visita `DisplayCategoryPicture.aspx?CategoryID=categoryID`, verrà generata un'eccezione che legge Impossibile per il cast dell'oggetto di tipo 'System. DBNull' nel tipo 'System. Byte []', sono presenti due elementi che possono causare questo. Prima di tutto, il `Categories` tabella s `Picture` colonna Consenti `NULL` valori. Il `DisplayCategoryPicture.aspx` pagina, si presuppone tuttavia, non è`NULL` valore presente. Il `Picture` proprietà del `CategoriesDataTable` può accedervi direttamente se dispone di un `NULL` valore. Se si desidera consentire `NULL` valori per il `Picture` colonna d da includere la condizione seguente:


[!code-vb[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample7.vb)]

Nel codice precedente si presuppone che in questo caso s alcuni immagine file denominato `NoPictureAvailable.gif` nel `Images` cartella che si desidera visualizzare le categorie senza un'immagine.

Questa eccezione può essere generata anche se il `CategoriesTableAdapter` s `GetCategoryWithBinaryDataByCategoryID` metodo s `SELECT` istruzione ha ripristinato l'elenco delle colonne query principale s, può verificarsi se si utilizza istruzioni SQL ad hoc e si sposta nuovamente la procedura guidata per i TableAdapter query principale. Verificare che `GetCategoryWithBinaryDataByCategoryID` metodo s `SELECT` istruzione include ancora il `Picture` colonna.

> [!NOTE]
> Ogni volta che il `DisplayCategoryPicture.aspx` viene visitato, accedere al database e vengono restituiti i dati di immagine specificato categoria s. Se t categoria s immagine ancora stato modificato dopo l'utente ha ultima visualizzazione, tuttavia, si tratta inutili. Fortunatamente, HTTP consente di *condizionale Ottiene*. Con una richiesta GET condizionale, il client effettua la richiesta HTTP invia lungo un [ `If-Modified-Since` intestazione HTTP](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html) che fornisce la data e ora ultima recuperato questa risorsa dal client al server web. Se il contenuto non è stato modificato dopo questa data specificata, il server web può rispondere con un [codice di stato non modificato (304)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) ed evitare l'invio di nuovo il contenuto della risorsa richiesta s. In breve, questa tecnica elimina il server web di dover inviare nuovamente il contenuto di una risorsa non è stato modificato dall'ultimo accesso, il client.


Per implementare questo comportamento, tuttavia, è necessario aggiungere un `PictureLastModified` colonna per il `Categories` tabella da cui acquisire quando il `Picture` nonché il codice per verificare la presenza di ultimo aggiornamento della colonna il `If-Modified-Since` intestazione. Per ulteriori informazioni sul `If-Modified-Since` intestazione e il flusso di lavoro GET condizionale, vedere [HTTP GET condizionale per hacker RSS](http://fishbowl.pastiche.org/2002/10/21/http_conditional_get_for_rss_hackers) e [più approfondita Cerca all'esecuzione di richieste HTTP in una pagina ASP.NET](http://aspnet.4guysfromrolla.com/articles/122204-1.aspx).

## <a name="step-4-displaying-the-category-pictures-in-a-gridview"></a>Passaggio 4: Visualizzare le immagini di categoria in un controllo GridView.

Ora che è disponibile una pagina web per visualizzare un'immagine di categoria s, è possibile visualizzare tramite il [controllo immagine Web](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/standard/image.aspx) o HTML `<img>` che punta all'elemento `DisplayCategoryPicture.aspx?CategoryID=categoryID`. Immagini, il cui URL è determinato dai dati del database possono essere visualizzate in GridView o utilizzando il ImageField DetailsView. Contiene il ImageField `DataImageUrlField` e `DataImageUrlFormatString` le proprietà che funzionano come s HyperLinkField `DataNavigateUrlFields` e `DataNavigateUrlFormatString` proprietà.

Consente di aumentare s il `Categories` GridView in `DisplayOrDownloadData.aspx` aggiungendo un ImageField per visualizzare ogni immagine categoria s. È sufficiente aggiungere il ImageField e impostare il relativo `DataImageUrlField` e `DataImageUrlFormatString` proprietà `CategoryID` e `DisplayCategoryPicture.aspx?CategoryID={0}`, rispettivamente. Verrà creata una colonna di GridView che esegue il rendering di un `<img>` elemento il cui `src` riferimenti dell'attributo `DisplayCategoryPicture.aspx?CategoryID={0}`, in cui verrà sostituita con la riga GridView s {0} `CategoryID` valore.


![Aggiungere un ImageField al controllo GridView.](displaying-binary-data-in-the-data-web-controls-vb/_static/image12.gif)

**Figura 12**: aggiungere un ImageField al controllo GridView.


Dopo aver aggiunto il ImageField, la sintassi dichiarativa di GridView s dovrebbe essere come soothe seguenti:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample8.aspx)]

Richiedere qualche istante per visualizzare questa pagina tramite un browser. Si noti come ogni record include ora un'immagine per la categoria.


[![La categoria s immagine viene visualizzata per ogni riga](displaying-binary-data-in-the-data-web-controls-vb/_static/image13.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image19.png)

**Figura 13**: la categoria s immagine viene visualizzata per ogni riga ([fare clic per visualizzare l'immagine ingrandita](displaying-binary-data-in-the-data-web-controls-vb/_static/image20.png))


## <a name="summary"></a>Riepilogo

In questa esercitazione sono state esaminate come presentare i dati binari. La modalità di presentazione dei dati dipende dal tipo di dati. Per i file PDF brochure è offerto all'utente una Brochure vista collegamento che, quando si fa clic, ha richiesto all'utente direttamente al file PDF. Per l'immagine di s categoria, è innanzitutto creata una pagina per recuperare e restituire i dati binari dal database e quindi utilizzare tale pagina per visualizzare ogni immagine s categoria in un controllo GridView.

Ora che si va esaminato come visualizzare i dati binari, è nuovamente pronto per esaminare come eseguire l'inserimento, aggiornamento ed eliminazione sul database con i dati binari. Nella prossima esercitazione verrà esaminato come associare un file caricato con i record del database corrispondente. Nell'esercitazione dopo che vedremo come aggiornare i dati binari esistenti, nonché come eliminare i dati binari quando viene rimosso il record associato.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Lead revisori per questa esercitazione sono stati Teresa Murphy e Dave Gardner. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](uploading-files-vb.md)
> [Successivo](including-a-file-upload-option-when-adding-a-new-record-vb.md)
