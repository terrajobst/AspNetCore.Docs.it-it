---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/nested-data-web-controls-cs
title: Controlli (c#) di dati nidificati Web | Documenti Microsoft
author: rick-anderson
description: "In questa esercitazione che verrà illustrato come utilizzare un ripetitore annidate all'interno di un altro controllo Repeater. Negli esempi verranno illustrato come popolare Ripetitore interno sia d..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: ad3cb0ec-26cf-42d7-b81b-184a34ec9f86
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/nested-data-web-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 69fa0489ff8baed1423d29ee7bfaa3157d35a76b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="nested-data-web-controls-c"></a>Controlli Web dati annidati (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare App di esempio](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_32_CS.exe) o [Scarica il PDF](nested-data-web-controls-cs/_static/datatutorial32cs1.pdf)

> In questa esercitazione che verrà illustrato come utilizzare un ripetitore annidate all'interno di un altro controllo Repeater. Negli esempi verranno illustrato come popolare Ripetitore interno in modo dichiarativo e a livello di codice.


## <a name="introduction"></a>Introduzione

Oltre a HTML statico e sintassi di associazione dati, i modelli possono includere anche i controlli Web e controlli utente. I controlli Web possono avere le proprietà assegnato tramite la sintassi di associazione dati dichiarativa, oppure è possibile accedere a livello di codice nei gestori di eventi sul lato server appropriato.

Incorporando controlli all'interno di un modello, è possibile personalizzare e migliorare l'aspetto e l'esperienza utente. Ad esempio, nel [TemplateFields utilizzo nel controllo GridView](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) dell'esercitazione, è stato illustrato come personalizzare la visualizzazione di s GridView mediante l'aggiunta di un controllo di calendario in un TemplateField per visualizzare la data di assunzione s di un dipendente, nel [aggiunta I controlli di convalida per la modifica e inserimento di interfacce](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) e [personalizzazione dell'interfaccia di modifica dati](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) esercitazioni, è stato illustrato come personalizzare il processo di modifica e le interfacce di inserimento tramite l'aggiunta di convalida controlli caselle di testo, controlli DropDownList e altri controlli Web.

Anche i modelli possono contenere altri controlli Web di dati. In, è possibile avere DataList che contiene un altro DataList (o Repeater o GridView o DetailsView e così via) all'interno dei modelli. La sfida tale interfaccia è associato i dati appropriati per i dati interni di controllo Web. Sono disponibili due diversi approcci, compreso fra dichiarative opzioni utilizzando ObjectDataSource a quelle a livello di codice.

In questa esercitazione che verrà illustrato come utilizzare un ripetitore annidate all'interno di un altro controllo Repeater. Ripetitore outer conterrà un elemento per ogni categoria nel database, visualizzare il nome della categoria s e la descrizione. Ogni elemento di categoria s Ripetitore interna verrà visualizzate le informazioni per ogni prodotto appartenenti alla categoria selezionata (vedere la figura 1) in un elenco puntato. Negli esempi verranno illustrato come popolare Ripetitore interno in modo dichiarativo e a livello di codice.


[![Vengono elencate tutte le categorie, insieme ai relativi prodotti,](nested-data-web-controls-cs/_static/image2.png)](nested-data-web-controls-cs/_static/image1.png)

**Figura 1**: ogni categoria, insieme ai relativi prodotti, sono elencati ([fare clic per visualizzare l'immagine ingrandita](nested-data-web-controls-cs/_static/image3.png))


## <a name="step-1-creating-the-category-listing"></a>Passaggio 1: Creare l'elenco di categorie

Quando la creazione di una pagina che utilizza nidificati dei controlli Web, utile per progettare, creare e testare il controllo Web di dati più esterna in primo luogo, senza preoccuparsi anche il controllo annidato interno. Pertanto, consentire s iniziare esaminando i passaggi necessari per aggiungere un controllo Repeater alla pagina che visualizza il nome e una descrizione per ogni categoria.

Avvia aprendo il `NestedControls.aspx` nella pagina di `DataListRepeaterBasics` cartella e aggiungere un controllo Repeater alla pagina, l'impostazione relativa `ID` proprietà `CategoryList`. Lo smart tag Ripetitore s, scegliere di creare un nuovo oggetto ObjectDataSource denominato `CategoriesDataSource`.


[![Denominare il nuovo ObjectDataSource CategoriesDataSource](nested-data-web-controls-cs/_static/image5.png)](nested-data-web-controls-cs/_static/image4.png)

**Figura 2**: nome nuovo ObjectDataSource `CategoriesDataSource` ([fare clic per visualizzare l'immagine ingrandita](nested-data-web-controls-cs/_static/image6.png))


Configurare ObjectDataSource in modo che riporta i dati di `CategoriesBLL` classe s `GetCategories` metodo.


[![Configurare ObjectDataSource per utilizzare il metodo GetCategories CategoriesBLL classe s](nested-data-web-controls-cs/_static/image8.png)](nested-data-web-controls-cs/_static/image7.png)

**Figura 3**: configurare ObjectDataSource per utilizzare il `CategoriesBLL` classe s `GetCategories` metodo ([fare clic per visualizzare l'immagine ingrandita](nested-data-web-controls-cs/_static/image9.png))


Per specificare il modello del ripetitore s contenuto è necessario passare alla visualizzazione origine e di immettere manualmente la sintassi dichiarativa. Aggiungere un `ItemTemplate` che visualizza il nome di categoria s in un `<h4>` elemento e la descrizione della categoria s in un elemento paragrafo (`<p>`). Inoltre, s consentono di separare ogni categoria con una regola orizzontale (`<hr>`). Dopo aver apportato queste modifiche nella pagina deve contenere la sintassi dichiarativa per il controllo Repeater e ObjectDataSource che è simile al seguente:


[!code-aspx[Main](nested-data-web-controls-cs/samples/sample1.aspx)]

La figura 4 Mostra stato di avanzamento quando viene visualizzato tramite un browser.


[![Ogni nome di categoria e la descrizione è elencato, separati da una regola orizzontale](nested-data-web-controls-cs/_static/image11.png)](nested-data-web-controls-cs/_static/image10.png)

**Figura 4**: è elencato ogni categoria s, nome e descrizione, separati da una regola orizzontale ([fare clic per visualizzare l'immagine ingrandita](nested-data-web-controls-cs/_static/image12.png))


## <a name="step-2-adding-the-nested-product-repeater"></a>Passaggio 2: Aggiunta di Repeater prodotto annidati

Con la categoria di visualizzazione di un elenco completo, l'attività successiva consiste nell'aggiungere un controllo Repeater per il `CategoryList` s `ItemTemplate` che visualizza informazioni su tali prodotti appartenenti alla categoria appropriata. Esistono diversi modi, che è possibile recuperare i dati per il controllo Repeater interno, due dei quali si esamineranno a breve. Per il momento, s consentono di creare solo i prodotti Ripetitore all'interno di `CategoryList` Ripetitore s `ItemTemplate`. In particolare, consentono s hanno visualizzato Ripetitore ogni prodotto in un elenco puntato con ogni elemento elenco inclusi il nome del prodotto s e il prezzo del prodotto.

Per creare il controllo Repeater è necessario immettere manualmente le sintassi dichiarativa per Ripetitore s interna e i modelli nel `CategoryList` s `ItemTemplate`. Aggiungere il markup seguente all'interno di `CategoryList` Ripetitore s `ItemTemplate`:


[!code-aspx[Main](nested-data-web-controls-cs/samples/sample2.aspx)]

## <a name="step-3-binding-the-category-specific-products-to-the-productsbycategorylist-repeater"></a>Passaggio 3: Associazione di specifiche della categoria di prodotti a Repeater ProductsByCategoryList

Se si visita la pagina tramite un browser a questo punto, la schermata avranno lo stesso aspetto come illustrato nella figura 4 perché è ve ancora per associare i dati al controllo Repeater. Esistono alcuni modi che è possibile acquisire i record di prodotto appropriate e associarle a Repeater, alcune più efficiente rispetto ad altri. In questo caso il problema principale è ottenere i prodotti appropriati per la categoria specificata.

I dati da associare al controllo Repeater interno sia accessibile in modo dichiarativo, tramite un oggetto ObjectDataSource il `CategoryList` Ripetitore s `ItemTemplate`, o a livello di codice, la pagina code-behind di pagine ASP.NET. Analogamente, questi dati possono essere associati a Repeater interno sia in modo dichiarativo - tramite Ripetitore interno s `DataSourceID` proprietà o tramite la sintassi per l'associazione dati dichiarativa o a livello di codice facendo riferimento Ripetitore interno nel `CategoryList` Ripetitore s `ItemDataBound` gestore dell'evento, l'impostazione a livello di codice relativo `DataSource` proprietà e la chiamata relativo `DataBind()` metodo. Consente di esplorare ognuno di questi approcci s.

## <a name="accessing-the-data-declaratively-with-an-objectdatasource-control-and-theitemdataboundevent-handler"></a>L'accesso ai dati in modo dichiarativo con un controllo ObjectDataSource e`ItemDataBound`gestore dell'evento

Poiché è già stato utilizzato diffusamente in tutta questa serie di esercitazioni, la scelta più naturale per l'accesso ai dati per questo esempio consiste nell'utilizzare ObjectDataSource ObjectDataSource. Il `ProductsBLL` classe dispone di un `GetProductsByCategoryID(categoryID)` metodo che restituisce informazioni su tali prodotti che appartengono all'oggetto specificato  *`categoryID`* . Pertanto, è possibile aggiungere un ObjectDataSource per la `CategoryList` Ripetitore s `ItemTemplate` e configurarlo per accedere ai dati da questo metodo di classe s.

Sfortunatamente, t Ripetitore consentono i modelli per la modifica tramite la visualizzazione di progettazione, quindi è necessario aggiungere manualmente la sintassi dichiarativa per questo controllo ObjectDataSource. Il sintassi seguente viene illustrata la `CategoryList` Ripetitore s `ItemTemplate` dopo l'aggiunta di questo nuovo ObjectDataSource (`ProductsByCategoryDataSource`):


[!code-aspx[Main](nested-data-web-controls-cs/samples/sample3.aspx)]

Quando si utilizza l'approccio ObjectDataSource è necessario impostare il `ProductsByCategoryList` Ripetitore s `DataSourceID` proprietà per il `ID` di ObjectDataSource (`ProductsByCategoryDataSource`). Inoltre, si noti che il nostro ObjectDataSource ha un `<asp:Parameter>` elemento che specifica il  *`categoryID`*  valore che verrà passati il `GetProductsByCategoryID(categoryID)` metodo. Ma, come si può specificare questo valore? Idealmente, d è in grado di impostare semplicemente il `DefaultValue` proprietà del `<asp:Parameter>` elemento utilizzando la sintassi di associazione dati, come illustrato di seguito:


[!code-aspx[Main](nested-data-web-controls-cs/samples/sample4.aspx)]

Purtroppo, l'associazione dati sintassi è valida solo nei controlli che hanno un `DataBinding` evento. La `Parameter` classe non dispone di un evento e pertanto la sintassi precedente non è valida e verrà generato un errore di runtime.

Per impostare questo valore, è necessario creare un gestore eventi per il `CategoryList` Ripetitore s `ItemDataBound` evento. Tenere presente che il `ItemDataBound` evento viene generato una volta per ogni elemento associato al controllo Repeater. Pertanto, ogni volta che questo evento viene generato per Ripetitore esterno è possibile assegnare corrente `CategoryID` valore per il `ProductsByCategoryDataSource` ObjectDataSource s `CategoryID` parametro.

Creare un gestore eventi per il `CategoryList` Ripetitore s `ItemDataBound` evento con il codice seguente:


[!code-csharp[Main](nested-data-web-controls-cs/samples/sample5.cs)]

Questo gestore eventi avvia assicurando che abbiamo re la gestione di un dato elemento anziché l'elemento di intestazione, piè di pagina o separatori. Successivamente, si fa riferimento l'effettivo `CategoriesRow` istanza in cui è appena stato associato all'oggetto corrente `RepeaterItem`. Infine, si fa riferimento in ObjectDataSource il `ItemTemplate` e assegnare il `CategoryID` valore del parametro per il `CategoryID` corrente `RepeaterItem`.

A questo gestore eventi, il `ProductsByCategoryList` Ripetitore in ogni `RepeaterItem` è associato a tali prodotti di `RepeaterItem` categoria s. Figura 5 mostra una schermata di output risultante.


[![Ripetitore Outer Elenca ogni categoria. Quello interno sono elencati i prodotti per categoria](nested-data-web-controls-cs/_static/image14.png)](nested-data-web-controls-cs/_static/image13.png)

**Figura 5**: il Ripetitore Elenca ogni categoria esterno; gli elenchi uno interna i prodotti per categoria ([fare clic per visualizzare l'immagine ingrandita](nested-data-web-controls-cs/_static/image15.png))


## <a name="accessing-the-products-by-category-data-programmatically"></a>Accesso a livello di codice i prodotti dai dati di categoria

Anziché ObjectDataSource per recuperare i prodotti per la categoria corrente, non è stato possibile creare un metodo nella classe code-behind di pagine ASP.NET (o il `App_Code` cartella o in un progetto libreria di classi separato) che restituisce il set appropriato di prodotti quando viene passato una `CategoryID`. Si supponga che questo metodo è stato nella classe code-behind di pagine ASP.NET e che era denominato `GetProductsInCategory(categoryID)`. Con questo metodo sul posto è stato possibile associare i prodotti per la categoria corrente Ripetitore interno utilizzando la sintassi dichiarativa seguente:


[!code-aspx[Main](nested-data-web-controls-cs/samples/sample6.aspx)]

Ripetitore s `DataSource` proprietà viene utilizzata la sintassi di associazione dati per indicare che i dati provengono dal `GetProductsInCategory(categoryID)` metodo. Poiché `Eval("CategoryID")` restituisce un valore di tipo `Object`, si esegue il cast dell'oggetto da un `Integer` prima di passarlo nel `GetProductsInCategory(categoryID)` metodo. Si noti che il `CategoryID` a cui si accede tramite l'associazione dati sintassi Ecco la `CategoryID` nel *outer* Ripetitore (`CategoryList`), quello che s associato ai record nel `Categories` tabella. Pertanto, è noto che `CategoryID` non può essere un database `NULL` valore, perché è ciecamente per eseguire il cast di `Eval` metodo senza verificare se è re affrontare un `DBNull`.

Con questo approccio, è necessario creare il `GetProductsInCategory(categoryID)` (metodo) e recuperare il set appropriato di prodotti di base fornito  *`categoryID`* . È possibile eseguire questa operazione restituisce semplicemente il `ProductsDataTable` restituito dal `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` metodo. Consente di creare s il `GetProductsInCategory(categoryID)` metodo nella classe code-behind per il nostro `NestedControls.aspx` pagina. Eseguire questa operazione utilizzando il codice seguente:


[!code-csharp[Main](nested-data-web-controls-cs/samples/sample7.cs)]

Questo metodo crea semplicemente un'istanza di `ProductsBLL` (metodo) e restituisce i risultati del `GetProductsByCategoryID(categoryID)` (metodo). Si noti che il metodo deve essere contrassegnato come `Public` o `Protected`; se il metodo è contrassegnato come `Private`, non sarà accessibile dal codice dichiarativo s pagina ASP.NET.

Dopo aver apportato le modifiche a questa tecnica è nuovo, dedicare alcuni minuti per visualizzare la pagina tramite un browser. L'output deve essere identico a quello quando si utilizza ObjectDataSource e `ItemDataBound` approccio gestore dell'evento (vedere la figura 5 per visualizzare una schermata riportata di seguito).

> [!NOTE]
> Se può sembrare busywork per creare il `GetProductsInCategory(categoryID)` metodo nella classe di codice della pagina s ASP.NET. Dopo tutto, questo metodo crea un'istanza del `ProductsBLL` classe e restituisce i risultati del relativo `GetProductsByCategoryID(categoryID)` metodo. Perché non semplicemente chiamare questo metodo direttamente dalla sintassi del data binding nel Repeater interna, ad esempio: `DataSource='<%# ProductsBLL.GetProductsByCategoryID((int)(Eval("CategoryID"))) %>'`? Sebbene questa sintassi vinto funzionano con l'implementazione corrente del `ProductsBLL` classe (poiché il `GetProductsByCategoryID(categoryID)` è un metodo di istanza), è possibile modificare `ProductsBLL` per includere un valore statico `GetProductsByCategoryID(categoryID)` metodo o la classe includono un valore statico `Instance()` per restituire una nuova istanza di `ProductsBLL` classe.


Anche se tali modifiche si eliminano la necessità di `GetProductsInCategory(categoryID)` metodo la classe code-behind s pagina ASP.NET, il metodo della classe di codice offre una maggiore flessibilità durante l'utilizzo con i dati recuperati, come si vedrà a breve.

## <a name="retrieving-all-of-the-product-information-at-once"></a>Recupero di tutte le informazioni di prodotto in una sola volta

Le due tecniche precedente è ve esaminate agganciare i prodotti per la categoria corrente eseguendo una chiamata al `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` (metodo) (il primo approccio abbiano tramite ObjectDataSource, il secondo e il `GetProductsInCategory(categoryID)` metodo nel classe code-behind). Ogni volta che viene richiamato questo metodo, le chiamate a livello di logica di Business a livello di accesso ai dati delle query con un'istruzione SQL che restituisce righe dal database di `Products` tabella i cui `CategoryID` campo corrisponde al parametro di input fornito.

Dato *N* viene separata di categorie nel sistema, questo approccio *N* + 1 chiamate a query di un database del database per ottenere tutte le categorie e quindi *N* chiamate per ottenere i prodotti specifico per ogni categoria. Tuttavia, è possibile, recuperare tutti i dati necessari nel database solo due chiamate una chiamata per ottenere tutte le categorie e l'altro per ottenere tutti i prodotti. Una volta che tutti i prodotti, è possibile filtrare i prodotti in modo che solo i prodotti corrispondenti corrente `CategoryID` sono associati a tale categoria s Ripetitore interna.

Per fornire questa funzionalità, è necessario apportare solo una lieve modifica per il `GetProductsInCategory(categoryID)` metodo la classe code-behind di pagine ASP.NET. Anziché ciecamente restituzione dei risultati del `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` (metodo), è possibile invece accedere innanzitutto *tutti* dei prodotti (se sono ancora non stata accede già) e quindi restituire solo la visualizzazione filtrata del i prodotti in base a passato `CategoryID`.


[!code-csharp[Main](nested-data-web-controls-cs/samples/sample8.cs)]

Si noti l'aggiunta della variabile a livello di pagina, `allProducts`. Contiene informazioni su tutti i prodotti e viene popolata la prima volta il `GetProductsInCategory(categoryID)` metodo viene richiamato. Dopo aver verificato che il `allProducts` oggetto è stato creato e popolato, il metodo filtra i risultati di s DataTable in modo che solo le righe il cui `CategoryID` corrisponde al valore specificato `CategoryID` sono accessibili. Questo approccio riduce il numero di volte in cui si accede al database da *N* + 1 fino a due.

Questa funzionalità avanzata non introduce modifiche al markup sottoposto a rendering della pagina, né portare nuovamente un numero di record rispetto a quella di altri. Semplicemente riduce il numero di chiamate al database.

> [!NOTE]
> Uno potrebbe intuitivamente motivo che la riduzione del numero di accessi al database sarebbe assuredly miglioramento delle prestazioni. Tuttavia, ciò potrebbe non essere il caso. Se si dispone di un numero elevato di prodotti il cui `CategoryID` è `NULL`, ad esempio, la chiamata al `GetProducts` metodo restituisce un numero di prodotti che non vengono mai visualizzati. Restituzione di tutti i prodotti, inoltre, può essere dispendiosa se si stanno Visualizza solo un subset delle categorie, che potrebbe essere il caso se è stato implementato il paging.


Come sempre, quando si tratta di analisi delle prestazioni delle due tecniche, la misura solo surefire consiste nell'eseguire test controllati personalizzate per gli scenari dell'applicazione s comuni case.

## <a name="summary"></a>Riepilogo

In questa esercitazione abbiamo visto come annidare i dati di un controllo Web all'interno di un altro, in particolare esaminare le procedure visualizzare un elemento per ogni categoria con un ripetitore interna elenco dei prodotti per ogni categoria in un elenco puntato un ripetitore esterno. La sfida principale nella creazione di un'interfaccia utente annidati consiste nell'accesso e l'associazione di dati corretti per il controllo Web dati interna. Esistono diverse tecniche disponibili, che due dei quali sono state esaminate in questa esercitazione. Il primo approccio esaminato utilizzato ObjectDataSource nei dati esterni controllo Web s `ItemTemplate` che è stata associata al controllo Web dati interno tramite il relativo `DataSourceID` proprietà. La seconda tecnica di accedere a dati tramite un metodo nella classe code-behind s pagina ASP.NET. Questo metodo può quindi essere associato ai dati interni controllo Web s `DataSource` proprietà tramite la sintassi per l'associazione dati.

Mentre l'interfaccia utente annidato esaminata in questa esercitazione è utilizzato un ripetitore annidato all'interno di un controllo Repeater, queste tecniche possono essere esteso ad altri controlli Web di dati. È possibile nidificare un ripetitore all'interno di un controllo GridView o un controllo GridView all'interno di DataList e così via.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Lead revisori per questa esercitazione sono stati Zack Jones e Liz Shulok. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Precedente](showing-multiple-records-per-row-with-the-datalist-control-cs.md)
[Successivo](displaying-data-with-the-datalist-and-repeater-controls-vb.md)
