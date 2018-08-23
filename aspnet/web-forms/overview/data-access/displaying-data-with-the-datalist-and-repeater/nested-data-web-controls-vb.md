---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/nested-data-web-controls-vb
title: Controlli (VB) Web dei dati annidati | Microsoft Docs
author: rick-anderson
description: In questa esercitazione che verrà presa in esame come usare un controllo Repeater annidate all'interno di un altro controllo Repeater. Gli esempi verranno illustrato come popolare il Repeater interno entrambi d...
ms.author: riande
ms.date: 09/13/2006
ms.assetid: 8b7fcf7b-722b-498d-a4e4-7c93701e0c95
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/nested-data-web-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 297d76da5bf049ec68a351562f96f3587b059b55
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831317"
---
<a name="nested-data-web-controls-vb"></a>Controlli Web dei dati annidati (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'App di esempio](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_32_VB.exe) o [Scarica il PDF](nested-data-web-controls-vb/_static/datatutorial32vb1.pdf)

> In questa esercitazione che verrà presa in esame come usare un controllo Repeater annidate all'interno di un altro controllo Repeater. Gli esempi illustrerà come popolare il Repeater interno in modo dichiarativo sia a livello di codice.


## <a name="introduction"></a>Introduzione

Oltre a codice HTML statico e sintassi di associazione dati, i modelli possono includere anche i controlli Web e controlli utente. Questi controlli Web possono avere le relative proprietà assegnate tramite sintassi di associazione dati dichiarativa, oppure è possibile accedere a livello di codice nei gestori di eventi sul lato server appropriato.

Incorporando i controlli all'interno di un modello, è possibile personalizzare l'aspetto e l'esperienza utente e migliorato. Ad esempio, nel [uso di TemplateFields nel controllo GridView](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) tutorial, è stato illustrato come personalizzare la visualizzazione di s GridView mediante l'aggiunta di un controllo di calendario in un TemplateField per visualizzare la data di assunzione s un dipendente, nel [aggiunta I controlli di convalida per la modifica e inserimento di interfacce](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) e [personalizzazione dell'interfaccia di modifica dati](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) esercitazioni, è stato illustrato come personalizzare il processo di modifica e inserimento interfacce mediante l'aggiunta di convalida controlli caselle di testo, controlli DropDownList e altri controlli di Web.

I modelli possono inoltre contenere altri controlli Web dei dati. Vale a dire godere di un controllo DataList che contiene un altro DataList (o Repeater o GridView o DetailsView e così via) all'interno dei modelli. La sfida con tale interfaccia sta effettuando l'associazione dei dati appropriati per i controllo Web per dati interni. Sono disponibili due diversi approcci, compreso tra le opzioni dichiarative utilizzando ObjectDataSource a quelli a livello di codice.

In questa esercitazione che verrà presa in esame come usare un controllo Repeater annidate all'interno di un altro controllo Repeater. Il Repeater outer conterrà un elemento per ogni categoria nel database, visualizzare il nome della categoria s e una descrizione. Ogni elemento di categoria s Repeater interni verranno visualizzate informazioni per ogni prodotto che appartengono alla categoria (vedere la figura 1) in un elenco puntato. Questi esempi verranno illustrato come popolare il Repeater interno in modo dichiarativo sia a livello di codice.


[![Ogni categoria, insieme ai propri prodotti, sono elencati](nested-data-web-controls-vb/_static/image2.png)](nested-data-web-controls-vb/_static/image1.png)

**Figura 1**: ogni categoria, insieme ai propri prodotti, sono elencati ([fare clic per visualizzare l'immagine con dimensioni normali](nested-data-web-controls-vb/_static/image3.png))


## <a name="step-1-creating-the-category-listing"></a>Passaggio 1: Creare l'elenco di categorie

Durante la creazione di una pagina che utilizza annidati controlli Web dei dati, risultare utili per progettare, creare e testare il controllo Web di dati più esterna in primo luogo, senza preoccuparsi anche il controllo annidato interno. Pertanto, consentire s avviare completando i passaggi necessari per aggiungere un controllo Repeater alla pagina che elenca il nome e una descrizione per ogni categoria.

Iniziare aprendo il `NestedControls.aspx` nella pagina la `DataListRepeaterBasics` cartella e aggiungere un controllo Repeater alla pagina, l'impostazione relativa `ID` proprietà `CategoryList`. Nello smart tag, Repeater s scegliere di creare un nuovo oggetto ObjectDataSource denominato `CategoriesDataSource`.


[![Denominare il nuovo ObjectDataSource CategoriesDataSource](nested-data-web-controls-vb/_static/image5.png)](nested-data-web-controls-vb/_static/image4.png)

**Figura 2**: assegnare un nome nuovo oggetto ObjectDataSource `CategoriesDataSource` ([fare clic per visualizzare l'immagine con dimensioni normali](nested-data-web-controls-vb/_static/image6.png))


Configurare ObjectDataSource in modo che effettua il pull dei relativi dati dal `CategoriesBLL` classe s `GetCategories` (metodo).


[![Configurare ObjectDataSource per usare il metodo GetCategories CategoriesBLL classe s](nested-data-web-controls-vb/_static/image8.png)](nested-data-web-controls-vb/_static/image7.png)

**Figura 3**: configurare ObjectDataSource per usare il `CategoriesBLL` classe s `GetCategories` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](nested-data-web-controls-vb/_static/image9.png))


Per specificare il modello del ripetitore s contenuto è necessario passare alla visualizzazione origine e di immettere manualmente la sintassi dichiarativa. Aggiungere un `ItemTemplate` che visualizza il nome della categoria s in un `<h4>` elemento e la descrizione della categoria s in un elemento paragrafo (`<p>`). Inoltre, ti permettono di s separare ogni categoria con una regola orizzontale (`<hr>`). Dopo aver apportato queste modifiche della pagina deve contenere la sintassi dichiarativa per il controllo Repeater e ObjectDataSource corrispondente è simile al seguente:


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample1.aspx)]

Figura 4 mostra lo stato di avanzamento quando viene visualizzato tramite un browser.


[![Viene elencato ogni nome di categoria e una descrizione, separati da una regola orizzontale](nested-data-web-controls-vb/_static/image11.png)](nested-data-web-controls-vb/_static/image10.png)

**Figura 4**: è elencata ogni categoria s nome e descrizione, separati da una regola orizzontale ([fare clic per visualizzare l'immagine con dimensioni normali](nested-data-web-controls-vb/_static/image12.png))


## <a name="step-2-adding-the-nested-product-repeater"></a>Passaggio 2: Aggiunta di Repeater prodotto annidati

Con la categoria di visualizzazione di un elenco completo, l'attività successiva consiste nell'aggiungere un controllo Repeater per la `CategoryList` s `ItemTemplate` che visualizza informazioni su tali prodotti appartenenti alla categoria appropriata. Esistono diversi modi, che è possibile recuperare i dati per il controllo Repeater interno, due dei quali verrà illustrato tra breve. Per ora, ti permettono di s creare solo i prodotti Repeater all'interno di `CategoryList` Repeater s `ItemTemplate`. In particolare, lasciare s che hanno visualizzato Repeater ogni prodotto in un elenco puntato con ogni elemento elenco tra cui il nome del prodotto s e il prezzo del prodotto.

Per creare il Repeater è necessario immettere manualmente le sintassi dichiarativa per il controllo Repeater s interna e i modelli nel `CategoryList` s `ItemTemplate`. Aggiungere il markup seguente all'interno di `CategoryList` Repeater s `ItemTemplate`:


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample2.aspx)]

## <a name="step-3-binding-the-category-specific-products-to-the-productsbycategorylist-repeater"></a>Passaggio 3: I prodotti specifiche della categoria di associazione a Repeater ProductsByCategoryList

Se si visita la pagina tramite un browser a questo punto, la schermata avrà un aspetto analogo a quello in figura 4 perché abbiamo ve ancora per associare tutti i dati al controllo Repeater. Esistono alcuni modi che è possibile recuperare i record di prodotto appropriate e associarle a Repeater, alcune più efficiente rispetto ad altri. In questo caso il problema principale è ricevendo i prodotti appropriati per la categoria specificata.

I dati da associare al controllo Repeater interno sono sia accessibile in modo dichiarativo, tramite un oggetto ObjectDataSource nel `CategoryList` Repeater s `ItemTemplate`, o a livello di codice dalla pagina code-behind ASP.NET pagina s. Analogamente, questi dati possono essere associati a Repeater interno sia in modo dichiarativo - tramite il Repeater interno s `DataSourceID` proprietà o tramite sintassi di associazione dati dichiarativa o a livello di codice facendo riferimento a Repeater interno nel `CategoryList` Repeater s `ItemDataBound` gestore eventi, l'impostazione a livello di codice relativi `DataSource` proprietà e chiamare il metodo relativo `DataBind()` (metodo). Let s esplorare ognuno di questi approcci.

## <a name="accessing-the-data-declaratively-with-an-objectdatasource-control-and-theitemdataboundevent-handler"></a>Accesso ai dati in modo dichiarativo con un controllo ObjectDataSource e`ItemDataBound`gestore dell'evento

Poiché è già stato utilizzato ObjectDataSource ampiamente in tutta questa serie di esercitazioni, la scelta più naturale per l'accesso ai dati per questo esempio consiste nell'utilizzare ObjectDataSource. Il `ProductsBLL` classe ha un `GetProductsByCategoryID(categoryID)` metodo che restituisce informazioni su tali prodotti che appartengono all'entità specificata *`categoryID`*. Pertanto, possiamo aggiungere ObjectDataSource per le `CategoryList` Repeater s `ItemTemplate` e configurarlo per accedere ai dati da questo metodo di classe s.

Sfortunatamente, il controllo Repeater Consen relativi modelli per la modifica tramite la visualizzazione di progettazione in modo che è necessario aggiungere manualmente la sintassi dichiarativa per il controllo ObjectDataSource. Il sintassi seguente sono illustrati i `CategoryList` Repeater s `ItemTemplate` dopo aver aggiunto il nuovo oggetto ObjectDataSource (`ProductsByCategoryDataSource`):


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample3.aspx)]

Quando si usa l'approccio di ObjectDataSource, dobbiamo impostare il `ProductsByCategoryList` Repeater s `DataSourceID` proprietà per il `ID` di ObjectDataSource (`ProductsByCategoryDataSource`). Inoltre, si noti che l'oggetto ObjectDataSource ha un `<asp:Parameter>` elemento che specifica il *`categoryID`* valore che verrà passati i `GetProductsByCategoryID(categoryID)` (metodo). Ma come si specifica questo valore? In teoria, d sarà in grado di impostare il `DefaultValue` proprietà del `<asp:Parameter>` elemento usando la sintassi di associazione dati, come illustrato di seguito:


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample4.aspx)]

Sfortunatamente, la sintassi di associazione dati è valida solo per i controlli che hanno un `DataBinding` evento. Il `Parameter` classe non dispone di tale evento e pertanto la sintassi precedente non è valida e verrà generato un errore di runtime.

Per impostare questo valore, è necessario creare un gestore eventi per il `CategoryList` Repeater s `ItemDataBound` evento. Si tenga presente che il `ItemDataBound` evento viene generato una volta per ogni elemento associato al controllo Repeater. Pertanto, ogni volta che questo evento viene generato per il controllo Repeater esterno è possibile assegnare l'oggetto corrente `CategoryID` valore per il `ProductsByCategoryDataSource` ObjectDataSource s `CategoryID` parametro.

Creare un gestore eventi per il `CategoryList` Repeater s `ItemDataBound` evento con il codice seguente:


[!code-vb[Main](nested-data-web-controls-vb/samples/sample5.vb)]

Questo gestore eventi viene avviato, garantendo che abbiamo re gestione di un dato elemento anziché l'elemento di intestazione, piè di pagina o un separatore. Successivamente, si fa riferimento l'oggetto effettivo `CategoriesRow` istanza che è appena stata associata all'oggetto corrente `RepeaterItem`. Infine, si fa riferimento l'oggetto ObjectDataSource nel `ItemTemplate` e assegnare relativi `CategoryID` valore del parametro per il `CategoryID` dell'oggetto corrente `RepeaterItem`.

A questo gestore eventi, il `ProductsByCategoryList` Repeater in ognuno `RepeaterItem` è associato a tali prodotti nel `RepeaterItem` categoria s. Figura 5 mostra una cattura di schermata dell'output risultante.


[![Outer Repeater elenca ogni categoria. Quello interno sono elencati i prodotti per categoria](nested-data-web-controls-vb/_static/image14.png)](nested-data-web-controls-vb/_static/image13.png)

**Figura 5**: il controllo Repeater elenca ogni categoria esterno; gli elenchi di un Inner i prodotti per categoria ([fare clic per visualizzare l'immagine con dimensioni normali](nested-data-web-controls-vb/_static/image15.png))


## <a name="accessing-the-products-by-category-data-programmatically"></a>L'accesso a individuare i prodotti per categoria dati a livello di codice

Anziché utilizzare ObjectDataSource per recuperare i prodotti per la categoria corrente, è possibile creare un metodo nella classe code-behind ASP.NET pagina s (o nel `App_Code` cartella o in un progetto libreria di classi separato) che restituisce il set appropriato di prodotti quando vengono passati un `CategoryID`. Si supponga che abbiamo dovuto tale metodo nella classe code-behind ASP.NET pagina s e che il file è stato denominato `GetProductsInCategory(categoryID)`. Con questo metodo posto è stato possibile associazione i prodotti per la categoria corrente a Repeater interno usando la sintassi dichiarativa per il seguente:


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample6.aspx)]

S Repeater `DataSource` proprietà viene utilizzata la sintassi di associazione dati per indicare che i relativi dati provengono dal `GetProductsInCategory(categoryID)` (metodo). Poiché `Eval("CategoryID")` restituisce un valore di tipo `Object`, si esegue il cast dell'oggetto da un `Integer` prima di passarlo nel `GetProductsInCategory(categoryID)` (metodo). Si noti che il `CategoryID` a cui si accede tramite l'associazione dati sintassi Ecco il `CategoryID` nel *outer* Repeater (`CategoryList`), quello che s associato ai record nel `Categories` tabella. Pertanto, sappiamo che la `CategoryID` non può essere un database `NULL` valore, ovvero il motivo per cui abbiamo alla cieca possiamo eseguire il cast di `Eval` metodo senza controllare se si ri affrontare un `DBNull`.

Con questo approccio, è necessario creare il `GetProductsInCategory(categoryID)` metodo e recuperare il set appropriato di prodotti ha fornito *`categoryID`*. È possibile eseguire questa operazione semplicemente restituire la `ProductsDataTable` restituiti dai `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` (metodo). Consente di creare s il `GetProductsInCategory(categoryID)` metodo nella classe code-behind per il `NestedControls.aspx` pagina. Eseguire questa operazione usando il codice seguente:


[!code-vb[Main](nested-data-web-controls-vb/samples/sample7.vb)]

Questo metodo crea semplicemente un'istanza del `ProductsBLL` metodo e restituisce i risultati del `GetProductsByCategoryID(categoryID)` (metodo). Si noti che il metodo deve essere contrassegnato `Public` oppure `Protected`; se il metodo è contrassegnato come `Private`, non sarà accessibile da markup dichiarativo s pagina ASP.NET.

Dopo aver apportato queste modifiche per usare questa tecnica di nuovo, si consiglia di visualizzare la pagina tramite un browser. L'output deve essere identica all'output quando si usa l'oggetto ObjectDataSource e `ItemDataBound` approccio gestore dell'evento (vedere la figura 5 per visualizzare una schermata riportata di seguito).

> [!NOTE]
> Potrebbe sembrare busywork per creare il `GetProductsInCategory(categoryID)` metodo nella classe code-behind ASP.NET pagina s. Dopo tutto, questo metodo crea un'istanza del `ProductsBLL` classe e restituisce i risultati del relativo `GetProductsByCategoryID(categoryID)` (metodo). Perché non semplicemente chiamare questo metodo direttamente dalla sintassi del data binding nel Repeater interna, ad esempio: `DataSource='<%# ProductsBLL.GetProductsByCategoryID(CType(Eval("CategoryID"), Integer)) %>'`? Sebbene la sintassi ha vinto lavoro t con l'implementazione corrente del `ProductsBLL` classe (poiché il `GetProductsByCategoryID(categoryID)` è un metodo di istanza), è possibile modificare `ProductsBLL` da includere un valore statico `GetProductsByCategoryID(categoryID)` metodo o che la classe includa un valore statico `Instance()` per restituire una nuova istanza di `ProductsBLL` classe.


Anche se queste modifiche avrebbe eliminato la necessità per il `GetProductsInCategory(categoryID)` metodo nella classe code-behind ASP.NET pagina s, il metodo della classe code-behind offre una maggiore flessibilità nell'utilizzo dei dati recuperati, come vedremo tra breve.

## <a name="retrieving-all-of-the-product-information-at-once"></a>Il recupero di tutte le informazioni sul prodotto in una sola volta

Le due tecniche precedente si va esaminato Scarica i prodotti per la categoria corrente eseguendo una chiamata al `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` (metodo) (il primo approccio ha tramite ObjectDataSource, il secondo e il `GetProductsInCategory(categoryID)` metodo nel classe code-behind). Ogni volta che viene richiamato questo metodo, le chiamate a Business Logic Layer fino a livello di accesso ai dati, che esegue una query con un'istruzione SQL che restituisce righe dal database di `Products` tabella i cui `CategoryID` campo corrisponde al parametro di input fornito.

Dato *N* viene separata di categorie nel sistema, questo approccio *N* + 1 chiamate a query di un database del database per ottenere tutte le categorie e quindi *N* chiamate per ottenere i prodotti specifico per ogni categoria. Tuttavia, è possibile, recuperare tutti i dati necessari in un'unica chiamata chiamate solo due database per ottenere tutte le categorie e l'altro per ottenere tutti i prodotti. Dopo aver ottenuto tutti i prodotti, è possibile filtrare i prodotti in modo che solo i prodotti corrispondenti corrente `CategoryID` sono associati a tale categoria s Repeater interna.

Per fornire questa funzionalità, è necessario apportare solo una modifica quasi irrilevante il `GetProductsInCategory(categoryID)` metodo nella classe code-behind ASP.NET pagina s. Invece di restituire alla cieca i risultati del `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` metodo, è possibile invece prima di tutto accedere *tutti* dei prodotti (se vengono eseguite t stato accede già) e quindi restituire solo la visualizzazione filtrata del prodotti di base passato `CategoryID`.


[!code-vb[Main](nested-data-web-controls-vb/samples/sample8.vb)]

Si noti l'aggiunta della variabile a livello di pagina, `allProducts`. Contiene informazioni su tutti i prodotti e viene popolata la prima volta il `GetProductsInCategory(categoryID)` metodo viene richiamato. Dopo aver verificato che il `allProducts` oggetto è stato creato e popolato, il metodo di filtrare i risultati di s DataTable in modo che solo le righe la cui proprietà `CategoryID` corrisponde al valore specificato `CategoryID` sono accessibili. Questo approccio riduce il numero di volte in cui il database è accessibile dalla *N* + 1 fino a due.

Questa funzionalità avanzata non introduce modifiche al markup sottoposto a rendering della pagina, né è caratterizzato da restituire meno record rispetto a altro approccio. È sufficiente riduce il numero di chiamate al database.

> [!NOTE]
> Uno potrebbe intuitivamente motivo che la riduzione del numero di accessi al database con certezza le prestazioni. Tuttavia, ciò potrebbe non essere il caso. Se si dispone di un numero elevato di prodotti il cui `CategoryID` viene `NULL`, ad esempio, la chiamata al `GetProducts` metodo restituisce un numero di prodotti che non vengono mai visualizzate. Inoltre, la restituzione di tutti i prodotti può essere dispendiosa se si ri Visualizza solo un subset delle categorie, che potrebbe essere il caso se è stato implementato il paging.


Come sempre, per quanto riguarda l'analisi delle prestazioni delle due tecniche seguenti, la misura solo surefire consiste nell'eseguire test controllati personalizzate per gli scenari di casi comuni di s dell'applicazione.

## <a name="summary"></a>Riepilogo

In questa esercitazione abbiamo visto come annidare i dati di un controllo Web in un altro, in particolare per esaminare come disporre un controllo Repeater esterno viene visualizzato un elemento per ogni categoria con un interno Repeater elenca i prodotti per ogni categoria in un elenco puntato. La sfida principale nella creazione di un'interfaccia utente annidati consiste nell'accesso e associazione dei dati corretti per il controllo Web per dati interna. Esistono varie tecniche disponibili, che due dei quali si sono esaminate in questa esercitazione. Il primo approccio esaminato usato ObjectDataSource nei dati controllo Web s outer `ItemTemplate` che è stata associata al controllo Web dei dati interne tramite relativo `DataSourceID` proprietà. La seconda tecnica di accedere ai dati tramite un metodo nella classe code-behind s pagina ASP.NET. Questo metodo può quindi essere associato a dati interni controllo Web s `DataSource` proprietà tramite la sintassi di associazione dati.

Mentre l'interfaccia utente annidato esaminato in questa esercitazione è usato un controllo Repeater annidati all'interno di un controllo Repeater, queste tecniche possono essere estese agli altri controlli Web dei dati. È possibile nidificare un controllo Repeater all'interno di un controllo GridView, o un controllo GridView all'interno di un controllo DataList e così via.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. I revisori per questa esercitazione sono state Zack Jones e Liz Shulok. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](showing-multiple-records-per-row-with-the-datalist-control-vb.md)
