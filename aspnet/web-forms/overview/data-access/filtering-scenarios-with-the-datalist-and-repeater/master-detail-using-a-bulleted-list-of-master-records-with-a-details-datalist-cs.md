---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs
title: Master-Details con un elenco puntato dei record Master DataList dettagli (c#) | Documenti Microsoft
author: rick-anderson
description: "In questa esercitazione si sarà comprimere il report di due pagine master-details dell'esercitazione precedente in una singola pagina, che mostra un elenco di nomi di categoria in t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2006
ms.topic: article
ms.assetid: c727bb73-7b59-41a1-8dc3-623c6d69e7c2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: 91b8139f082704c5b5964087cc1887454c081f09
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="masterdetail-using-a-bulleted-list-of-master-records-with-a-details-datalist-c"></a>Master-Details con un elenco puntato dei record Master DataList dettagli (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare App di esempio](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_35_CS.exe) o [Scarica il PDF](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/datatutorial35cs1.pdf)

> In questa esercitazione si sarà comprimere il report di due pagine master-details dell'esercitazione precedente in una singola pagina, che mostra un elenco di nomi di categoria sul lato sinistro dello schermo e i prodotti della categoria selezionata a destra della schermata.


## <a name="introduction"></a>Introduzione

Nel [esercitazione precedente](master-detail-filtering-acess-two-pages-datalist-cs.md) abbiamo esaminato come separare un report master/dettaglio tra due pagine. Nella pagina master è usato un controllo Repeater per il rendering di un elenco puntato di categorie. Ogni nome di categoria è un collegamento ipertestuale che, quando si fa clic, si accettano l'utente alla pagina dei dettagli, in cui un controllo DataList due colonne mostrano i prodotti appartenenti alla categoria selezionata.

In questa esercitazione si verrà comprimere l'esercitazione di due pagine in una singola pagina, che mostra un elenco di nomi di categoria sul lato sinistro della schermata con il nome di ogni categoria sottoposto a rendering come LinkButton. Facendo clic sul nome della categoria LinkButton provoca un postback e associa i prodotti s categoria selezionata DataList due colonne a destra della schermata. Oltre a visualizzare il nome di ogni categoria s, Ripetitore a sinistra mostra del numero totale di prodotti per una determinata categoria (vedere la figura 1).


[![La categoria s nome e il numero totale di prodotti vengono visualizzate a sinistra](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image2.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image1.png)

**Figura 1**: la categoria s, nome e numero totale di prodotti vengono visualizzate a sinistra ([fare clic per visualizzare l'immagine ingrandita](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image3.png))


## <a name="step-1-displaying-a-repeater-in-the-left-portion-of-the-screen"></a>Passaggio 1: Visualizzare un ripetitore nella parte sinistra della schermata

Per questa esercitazione è necessario disporre dell'elenco delle categorie vengono visualizzate a sinistra dei prodotti s categoria selezionata. Il contenuto all'interno di una pagina web può essere posizionato utilizzando tag HTML standard elementi paragrafo, gli spazi unificatori, `<table>` s e così via o tramite le tecniche di foglio di stile (CSS). Tutte le esercitazioni finora sono utilizzate le tecniche CSS per il posizionamento. Quando si compila l'interfaccia utente di spostamento in questa pagina master nel [pagine Master e esplorazione del sito](../introduction/master-pages-and-site-navigation-cs.md) esercitazione utilizzassimo *il posizionamento assoluto*, che indica l'offset del precisa pixel per la navigazione elenco e il contenuto principale. In alternativa, CSS può essere utilizzati per posizionare un elemento a destra o a sinistra di un altro tramite *mobile*. Possiamo abbiamo dell'elenco delle categorie vengono visualizzate a sinistra dei prodotti s categoria selezionata da Mobile Ripetitore a sinistra del controllo DataList

Aprire il `CategoriesAndProducts.aspx` pagina dal `DataListRepeaterFiltering` cartella e aggiungere alla pagina un ripetitore e un controllo DataList. Impostare il controllo Repeater s `ID` a `Categories` e DataList s per `CategoryProducts`. Passare alla visualizzazione origine e inserire i controlli Repeater e DataList entro i propri `<div>` elementi. Vale a dire, racchiudere Ripetitore all'interno di un `<div>` elemento prima e quindi DataList nel proprio `<div>` elemento direttamente dopo il controllo Repeater. Il markup a questo punto dovrebbe essere simile al seguente:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample1.aspx)]

Per rendere mobile Ripetitore a sinistra del controllo DataList, è necessario utilizzare il `float` attributo di stile CSS, come illustrato di seguito:


[!code-html[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample2.html)]

Il `float: left;` scorre il primo `<div>` elemento a sinistra del secondo. Il `width` e `padding-right` impostazioni indicano il primo `<div>` s `width` e viene aggiunta la spaziatura tra il `<div>` contenuto dell'elemento s e il margine destro. Per ulteriori informazioni su mobile elementi in CSS estrarre il [Floatutorial](http://css.maxdesign.com.au/floatutorial/).

Anziché specificare l'impostazione di stile direttamente tramite il primo `<p>` elemento s `style` attributo, consente di creare invece una nuova classe CSS in s `Styles.css` denominato `FloatLeft`:


[!code-css[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample3.css)]

Quindi è possibile sostituire il `<div>` con `<div class="FloatLeft">`.

Dopo aver aggiunto la classe CSS e il markup nella configurazione di `CategoriesAndProducts.aspx` page, passare alla finestra di progettazione. Dovrebbe essere Ripetitore a virgola mobile a sinistra del controllo DataList (sebbene destra entrambi semplicemente appariranno come le caselle di grigie poiché è ve ancora per configurare le origini dati o modelli).


[![Ripetitore è resa mobile a sinistra del controllo DataList](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image5.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image4.png)

**Figura 2**: il Ripetitore è resa mobile a sinistra del controllo DataList ([fare clic per visualizzare l'immagine ingrandita](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image6.png))


## <a name="step-2-determining-the-number-of-products-for-each-category"></a>Passaggio 2: Determinare il numero di prodotti per ogni categoria

S Repeater e DataList circostanti markup completo, è nuovamente pronto per associare i dati della categoria a Repeater di controllo. Tuttavia, come mostra l'elenco puntato delle categorie nella figura 1, oltre a ogni nome di categoria s è anche necessario visualizzare il numero di prodotti associati alla categoria. Per accedere a queste informazioni è possibile:

- **Consente di rilevare queste informazioni dalla classe code-behind s pagina ASP.NET.** Fornendo una particolare  *`categoryID`*  è possibile determinare il numero di prodotti associati chiamando il `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` metodo. Questo metodo restituisce un `ProductsDataTable` i cui `Count` proprietà indica quanti `ProductsRow` s esista, che corrisponde al numero di prodotti per l'oggetto specificato  *`categoryID`* . È possibile creare un `ItemDataBound` gestore eventi per il controllo Repeater che, per ogni categoria associato al controllo Repeater, chiama il `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` (metodo) e include il conteggio nell'output.
- **Aggiornamento di `CategoriesDataTable` del dataset tipizzato da includere un `NumberOfProducts` colonna.** È quindi possibile aggiornare il `GetCategories()` metodo il `CategoriesDataTable` per includere tali informazioni o, in alternativa, lasciare `GetCategories()` come-è e creare un nuovo `CategoriesDataTable` metodo chiamato `GetCategoriesAndNumberOfProducts()`.

Consente di esplorare entrambe le tecniche s. Il primo approccio è più semplice implementare poiché non abbiamo non è necessario aggiornare il livello di accesso ai dati; Tuttavia, è necessario le ulteriori comunicazioni con il database. La chiamata al `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` metodo il `ItemDataBound` gestore eventi aggiunge una chiamata di database aggiuntive per ogni categoria visualizzata nel Repeater. Con questa tecnica esistono *N* + chiamate di 1 database, dove *N* è il numero di categorie visualizzate nel Repeater. Con il secondo approccio, viene restituito il numero di prodotti con informazioni su ogni categoria di `CategoriesBLL` classe s `GetCategories()` (o `GetCategoriesAndNumberOfProducts()`) (metodo), determinando in tal modo solo un unico round trip al database.

## <a name="determining-the-number-of-products-in-the-itemdatabound-event-handler"></a>Determinazione del numero di prodotti nel gestore eventi ItemDataBound

Determinazione del numero di prodotti per ogni categoria nel Repeater s `ItemDataBound` gestore dell'evento non richiede alcuna modifica per il livello di accesso ai dati esistenti. Tutte le modifiche, è possono eseguire direttamente all'interno di `CategoriesAndProducts.aspx` pagina. Per iniziare, aggiungere un nuovo oggetto ObjectDataSource denominato `CategoriesDataSource` tramite lo smart tag Ripetitore s. A questo punto, configurare il `CategoriesDataSource` ObjectDataSource in modo che recupera i dati di `CategoriesBLL` classe s `GetCategories()` metodo.


[![Configurare ObjectDataSource per utilizzare la classe CategoriesBLL s GetCategories() (metodo)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image8.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image7.png)

**Figura 3**: configurare ObjectDataSource per utilizzare il `CategoriesBLL` classe s `GetCategories()` metodo ([fare clic per visualizzare l'immagine ingrandita](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image9.png))


Ogni elemento di `Categories` Ripetitore deve essere selezionabile e, quando si fa clic, che il `CategoryProducts` DataList per visualizzare i prodotti per la categoria selezionata. Questo può essere eseguito apportando ogni categoria di un collegamento ipertestuale, il collegamento a questa pagina stessa (`CategoriesAndProducts.aspx`), ma passando il `CategoryID` tramite il parametro querystring, proprio come abbiamo visto nell'esercitazione precedente. Il vantaggio di questo approccio è che una pagina contenente i prodotti di una particolare categoria s può essere rimossa e indicizzata dal motore di ricerca.

In alternativa, è possibile rendere ogni categoria LinkButton, che è l'approccio da usare per questa esercitazione. Esegue il rendering nel browser utente s come collegamento ipertestuale sull'elemento LinkButton ma, quando si fa clic, provoca un postback; durante il postback, s DataList ObjectDataSource deve essere aggiornato per visualizzare i prodotti appartenenti alla categoria selezionata. Per questa esercitazione, utilizzando un collegamento ipertestuale preferibile rispetto all'utilizzo di LinkButton; Tuttavia, potrebbero esserci altri scenari in cui è più vantaggioso utilizzare LinkButton. Mentre l'approccio di collegamento ipertestuale sarebbe ideale per questo esempio, consentire s alternativa all'utilizzo di LinkButton. Come si vedrà, utilizzando un LinkButton implica alcuni problemi che con un collegamento ipertestuale non risulterebbe in caso contrario. Utilizzando un LinkButton in questa esercitazione verrà evidenziare queste sfide pertanto consentono di fornire soluzioni per gli scenari in cui si può decidere di utilizzare LinkButton anziché un collegamento ipertestuale.

> [!NOTE]
> Vengono fornite informazioni utili per ripetere questa esercitazione utilizzando un controllo collegamento ipertestuale o `<a>` elemento anziché sull'elemento LinkButton.


Il markup seguente viene illustrata la sintassi dichiarativa per Ripetitore e ObjectDataSource. Si noti che i modelli di s Ripetitore eseguire il rendering di un elenco puntato con ogni elemento come LinkButton:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample4.aspx)]

> [!NOTE]
> Per questa esercitazione Ripetitore deve avere il proprio stato abilitato (si noti l'omissione del `EnableViewState="False"` dalla sintassi dichiarativa del ripetitore s). Nel passaggio 3 è verrà creato un gestore eventi per il controllo Repeater s `ItemCommand` evento in cui sarà possibile l'aggiornamento è DataList s s ObjectDataSource `SelectParameters` insieme. Ripetitore s `ItemCommand`, tuttavia, ha vinto incendio t Se lo stato di visualizzazione è disabilitato. Vedere [A Stumper di una domanda ASP.NET](http://scottonwriting.net/sowblog/posts/1263.aspx) e [la soluzione](http://scottonwriting.net/sowBlog/posts/1268.aspx) per ulteriori informazioni sui motivi per cui lo stato di visualizzazione deve essere abilitato per un ripetitore s `ItemCommand` dell'evento da generare.


Sull'elemento LinkButton con il `ID` valore della proprietà `ViewCategory` non dispone il `Text` set di proprietà. Se avessimo desideriamo visualizzare il nome di categoria, si sarebbe impostato la proprietà Text in modo dichiarativo, tramite la sintassi per l'associazione dati, come illustrato di seguito:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample5.aspx)]

Tuttavia, con cui si desidera mostrare sia il nome di categoria s *e* il numero di prodotti appartenenti alla categoria selezionata. Queste informazioni possono essere recuperate da Ripetitore s `ItemDataBound` effettuando una chiamata al gestore dell'evento di `ProductBLL` classe s `GetCategoriesByProductID(categoryID)` (metodo) e determinare il numero di record restituito nella finestra di `ProductsDataTable`, come nel codice seguente di seguito viene illustrato:


[!code-csharp[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample6.cs)]

Iniziamo assicurando che abbiamo re l'utilizzo di un elemento di dati (uno cui `ItemType` è `Item` o `AlternatingItem`) e quindi fare riferimento il `CategoriesRow` istanza in cui è appena stato associato all'oggetto corrente `RepeaterItem`. Successivamente, è determinare il numero di prodotti per questa categoria viene creata un'istanza del `ProductsBLL` classe, chiamata relativo `GetCategoriesByProductID(categoryID)` (metodo) e determinare il numero di record restituiti utilizzando il `Count` proprietà. Infine, il `ViewCategory` LinkButton in ItemTemplate è riferimenti e il relativo `Text` è impostata su *CategoryName* (*NumberOfProductsInCategory*), dove  *NumberOfProductsInCategory* è formattato come un numero con zero cifre decimali.

> [!NOTE]
> In alternativa, è possibile avere aggiunto un *formattazione funzione* per la classe code-behind di pagine ASP.NET che accetta una categoria s `CategoryName` e `CategoryID` valori e restituisce il `CategoryName` concatenato con il numero di Nella categoria di prodotti (come determinato chiamando il `GetCategoriesByProductID(categoryID)` (metodo)). I risultati di tale funzione formattazione può essere assegnati in modo dichiarativo a s LinkButton eliminando la necessità di proprietà di testo il `ItemDataBound` gestore dell'evento. Fare riferimento al [TemplateFields utilizzo nel controllo GridView](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) o [formattazione di DataList e basato su dati Ripetitore](../displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-cs.md) esercitazioni per ulteriori informazioni sull'utilizzo delle funzioni di formattazione.


Dopo aver aggiunto il gestore eventi, è opportuno testare la pagina tramite un browser. Si noti come ogni categoria è elencato in un elenco puntato, visualizzare il nome della categoria s e il numero di prodotti associati alla categoria (vedere la figura 4).


[![Ogni nome di categoria e il numero di prodotti vengono visualizzati.](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image11.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image10.png)

**Figura 4**: vengono visualizzate ogni categoria s, nome e numero di prodotti ([fare clic per visualizzare l'immagine ingrandita](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image12.png))


## <a name="updating-thecategoriesdatatableandcategoriestableadapterto-include-the-number-of-products-for-each-category"></a>L'aggiornamento di`CategoriesDataTable`e`CategoriesTableAdapter`per includere il numero di prodotti per ogni categoria

Invece di determinare il numero di prodotti per ogni categoria come s associato al controllo Repeater, è possibile semplificare questo processo regolando il `CategoriesDataTable` e `CategoriesTableAdapter` nel livello di accesso ai dati da includere queste informazioni in modo nativo. A tale scopo, è necessario aggiungere una nuova colonna a `CategoriesDataTable` per contenere il numero di prodotti associati. Per aggiungere una nuova colonna a un oggetto DataTable, aprire il DataSet tipizzato (`App_Code\DAL\Northwind.xsd`), fare clic su oggetto DataTable da modificare e scegliere Aggiungi / colonna. Aggiungere una nuova colonna per il `CategoriesDataTable` (vedere Figura 5).


[![Aggiungere una nuova colonna per il CategoriesDataSource](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image14.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image13.png)

**Figura 5**: aggiungere una nuova colonna per il `CategoriesDataSource` ([fare clic per visualizzare l'immagine ingrandita](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image15.png))


Verrà aggiunta una nuova colonna denominata `Column1`, che è possibile modificare semplicemente digitando un nome diverso. La nuova colonna rinominata `NumberOfProducts`. Successivamente, è necessario configurare le proprietà di questa colonna s. Fare clic su nuova colonna e passare alla finestra Proprietà. Modificare la colonna s `DataType` proprietà `System.String` a `System.Int32` e impostare il `ReadOnly` proprietà `True`, come illustrato nella figura 6.


![Impostare le proprietà di sola lettura della nuova colonna e il tipo di dati](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image16.png)

**Figura 6**: impostare il `DataType` e `ReadOnly` le proprietà della nuova colonna


Mentre il `CategoriesDataTable` include ora un `NumberOfProducts` colonna, il relativo valore non è impostato da una delle query s TableAdapter corrispondente. È possibile aggiornare il `GetCategories()` per restituire queste informazioni se si desiderano che tali informazioni restituito ogni volta che viene recuperate le informazioni di categoria. Se, tuttavia, è necessario solo acquisire il numero di prodotti associati per le categorie in rari casi, ad esempio solo per questa esercitazione, quindi, è possibile mantenere `GetCategories()` come-è e creare un nuovo metodo che restituisce queste informazioni. S consentono di utilizzare questa procedura, la creazione di un nuovo metodo denominato `GetCategoriesAndNumberOfProducts()`.

Per aggiungere questo nuovo `GetCategoriesAndNumberOfProducts()` destro del mouse sul metodo di `CategoriesTableAdapter` e scegliere Nuova Query. In questo modo up TableAdapter Query configurazione guidata, che si va utilizzata più volte nelle esercitazioni precedenti. Per questo metodo, avviare la procedura guidata, indicando che la query utilizza un'istruzione SQL ad hoc che restituisce righe.


[![Creare il metodo utilizzando un'istruzione SQL Ad Hoc](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image18.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image17.png)

**Figura 7**: creare il metodo usando un'istruzione SQL Ad Hoc ([fare clic per visualizzare l'immagine ingrandita](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image19.png))


[![L'istruzione SQL restituisce righe](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image21.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image20.png)

**Figura 8**: l'istruzione di SQL restituisce righe ([fare clic per visualizzare l'immagine ingrandita](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image22.png))


Schermata di procedura guidata richiede us per la query da utilizzare. Per ogni categoria s `CategoryID`, `CategoryName`, e `Description` campi, insieme al numero di prodotti associati alla categoria, utilizzano la seguente `SELECT` istruzione:


[!code-sql[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample7.sql)]


[![Specificare la Query da utilizzare](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image24.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image23.png)

**Figura 9**: specificare la Query da utilizzare ([fare clic per visualizzare l'immagine ingrandita](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image25.png))


Si noti che la sottoquery che calcola il numero di prodotti associati alla categoria è associato l'alias `NumberOfProducts`. La corrispondenza dei nomi, il valore restituito da questa sottoquery può essere associato il `CategoriesDataTable` s `NumberOfProducts` colonna.

Dopo aver immesso la query, l'ultimo passaggio consiste nello scegliere il nome per il nuovo metodo. Utilizzare `FillWithNumberOfProducts` e `GetCategoriesAndNumberOfProducts` per il riempimento del DataTable e restituire un oggetto DataTable modelli, rispettivamente.


[![Nome di metodi FillWithNumberOfProducts nuovo TableAdapter s e GetCategoriesAndNumberOfProducts](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image27.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image26.png)

**Figura 10**: assegnare un nome ai TableAdapter nuovi metodi `FillWithNumberOfProducts` e `GetCategoriesAndNumberOfProducts` ([fare clic per visualizzare l'immagine ingrandita](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image28.png))


A questo punto il livello di accesso ai dati è stato esteso per includere il numero di prodotti per categoria. È necessario aggiungere un corrispondente poiché tutto il livello di presentazione indirizza tutte le chiamate alla funzione tramite un livello di logica di Business separato DAL `GetCategoriesAndNumberOfProducts` metodo per la `CategoriesBLL` classe:


[!code-csharp[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample8.cs)]

Con DAL e BLL completa, è re pronti per l'associazione dati per il `Categories` Ripetitore in `CategoriesAndProducts.aspx`! Se è già creato già ObjectDataSource per Ripetitore da di determinare il numero di prodotti nel `ItemDataBound` sezione del gestore eventi, eliminare questo ObjectDataSource e rimuovere Ripetitore s `DataSourceID` proprietà impostazione; liberare dai cavi anche il Ripetitore s `ItemDataBound` evento dal gestore dell'evento rimuovendo il `Handles Categories.OnItemDataBound` sintassi nella classe code-behind ASP.NET.

Con il controllo Repeater nuovamente nello stato originale, aggiungere un nuovo oggetto ObjectDataSource denominato `CategoriesDataSource` tramite lo smart tag Ripetitore s. Configurare ObjectDataSource per utilizzare il `CategoriesBLL` (classe), ma anziché utilizzare il `GetCategories()` (metodo), hanno utilizzi `GetCategoriesAndNumberOfProducts()` invece (vedere Figura 11).


[![Configurare ObjectDataSource per utilizzare il metodo GetCategoriesAndNumberOfProducts](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image30.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image29.png)

**Figura 11**: configurare ObjectDataSource per utilizzare il `GetCategoriesAndNumberOfProducts` metodo ([fare clic per visualizzare l'immagine ingrandita](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image31.png))


Aggiornare quindi il `ItemTemplate` in modo che s LinkButton `Text` proprietà viene assegnata in modo dichiarativo utilizzando la sintassi di associazione dati e include sia il `CategoryName` e `NumberOfProducts` campi dati. Il markup dichiarativo completo per il controllo Repeater e `CategoriesDataSource` ObjectDataSource seguenti:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample9.aspx)]

L'output sottoposto a rendering mediante l'aggiornamento per includere un `NumberOfProducts` colonna equivale all'utilizzo di `ItemDataBound` approccio gestore dell'evento (vedere la figura 4 per visualizzare una schermata cattura del ripetitore con i nomi di categoria e un numero di prodotti).

## <a name="step-3-displaying-the-selected-category-s-products"></a>Passaggio 3: Visualizzare i prodotti s categoria selezionata

A questo punto si dispone di `Categories` Ripetitore la visualizzazione dell'elenco di categorie e il numero di prodotti in ogni categoria. Ripetitore utilizza LinkButton per ogni categoria che, quando si fa clic, comporta un postback, in corrispondenza del quale punto è necessario visualizzare i prodotti per la categoria selezionata nel `CategoryProducts` DataList.

Una sfida che ci viene illustrato come visualizzare solo i prodotti per la categoria selezionata in DataList. Nel [Master/dettaglio con Master GridView selezionabile DetailsView dettagli](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) esercitazione è stato illustrato come compilare un controllo GridView le cui righe potrebbe essere selezionata, con la riga selezionata s dettagli visualizzati in un controllo DetailsView nella stessa pagina. S GridView ObjectDataSource restituite informazioni su tutti i prodotti usando il `ProductsBLL` s `GetProducts()` il metodo s DetailsView ObjectDataSource recupera informazioni sull'utilizzo di un prodotto selezionato il `GetProductsByProductID(productID)` metodo. Il  *`productID`*  valore del parametro è stato specificato in modo dichiarativo mediante l'associazione con il valore di GridView s `SelectedValue` proprietà. Purtroppo, Ripetitore non dispone di un `SelectedValue` proprietà e non può essere utilizzato come origine parametro.

> [!NOTE]
> Questo è uno di tali sfide che vengono visualizzati quando si utilizza il LinkButton in un controllo Repeater. Se avessimo è utilizzato un collegamento ipertestuale per passare il `CategoryID` tramite il parametro querystring è invece possibile usare tale campo QueryString come origine per il valore del parametro s.


Prima di un problema è causato dalla mancanza di un `SelectedValue` proprietà per il controllo Repeater, tuttavia, consentono s prima di associare DataList ObjectDataSource e specificare il relativo `ItemTemplate`.

Smart tag DataList s, scegliere di aggiungere un nuovo oggetto ObjectDataSource denominato `CategoryProductsDataSource` e configurarlo per utilizzare il `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` metodo. Poiché in questa esercitazione DataList offre un'interfaccia di sola lettura, è possibile impostare gli elenchi a discesa nell'istruzione INSERT, UPDATE ed eliminare le tabulazioni su (nessuno).


[![Configurare ObjectDataSource per utilizzare la classe ProductsBLL s GetProductsByCategoryID(categoryID) metodo](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image33.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image32.png)

**Figura 12**: configurare ObjectDataSource per utilizzare `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` metodo ([fare clic per visualizzare l'immagine ingrandita](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image34.png))


Poiché il `GetProductsByCategoryID(categoryID)` metodo prevede un parametro di input (*`categoryID`*), la configurazione guidata origine dati consente di specificare l'origine di s parametri. Le categorie erano stato elencate in un controllo GridView o DataList, d impostiamo l'elenco di riepilogo a discesa Origine parametro al controllo e il ControlID per il `ID` dei dati di controllo Web. Tuttavia, poiché non è stato specificato il Ripetitore un `SelectedValue` proprietà non può essere utilizzato come un'origine del parametro. Se si seleziona, si noterà che l'elenco di riepilogo a discesa ControlID contiene solo un controllo `ID``CategoryProducts`, `ID` del controllo DataList.

Per il momento, impostare l'elenco di riepilogo a discesa Origine parametro su Nessuno. Al termine saremo assegnazione livello di codice il valore di questo parametro, quando una categoria a cui si fa clic su LinkButton nel Repeater.


[![Eseguire non specifica un parametro di origine per il parametro categoryID](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image36.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image35.png)

**Figura 13**: non si specifica un parametro di origine per il  *`categoryID`*  parametro ([fare clic per visualizzare l'immagine ingrandita](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image37.png))


Dopo aver completato la configurazione guidata origine dati, Visual Studio genera automaticamente DataList s `ItemTemplate`. Sostituire il valore predefinito `ItemTemplate` con il modello utilizzato nell'esercitazione precedente; inoltre, impostare DataList s `RepeatColumns` proprietà su 2. Dopo aver apportato queste modifiche dichiarativo del controllo DataList e ObjectDataSource associati dovrebbe essere simile al seguente:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample10.aspx)]

Attualmente, il `CategoryProductsDataSource` ObjectDataSource s  *`categoryID`*  parametro non viene impostato in modo prodotti non vengono visualizzati quando si visualizza la pagina. È necessario è impostato il valore del parametro in base il `CategoryID` della categoria selezionata nel Repeater. Ciò introduce due sfide: in primo luogo, come Microsoft determina quando LinkButton nel Repeater s `ItemTemplate` è stato fatto clic; e il secondo, come è possibile determinare il `CategoryID` della categoria corrispondente il cui LinkButton selezionato?

Sull'elemento LinkButton Analogamente ai controlli Button e ImageButton ha un `Click` evento e un [ `Command` evento](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.linkbutton.command.aspx). Il `Click` evento è progettato per semplicemente si noti che è stato fatto clic con il LinkButton. In alcuni casi, tuttavia, oltre a notare che è stato fatto clic con il LinkButton è anche necessario passare alcune informazioni aggiuntive per il gestore dell'evento. In questo caso, s LinkButton [ `CommandName` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.linkbutton.commandname.aspx) e [ `CommandArgument` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.linkbutton.commandargument.aspx) proprietà possono essere assegnate a queste informazioni aggiuntive. Quindi, quando si fa clic sull'elemento LinkButton, relativo `Command` viene generato l'evento (invece che relativo `Click` eventi), il gestore dell'evento verrà passato i valori del `CommandName` e `CommandArgument` proprietà.

Quando un `Command` evento viene generato dall'interno di un modello nel Repeater, s Ripetitore [ `ItemCommand` evento](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.repeater.itemcommand.aspx) viene generato e viene passato il `CommandName` e `CommandArgument` i valori del controllo LinkButton selezionato (o un pulsante o ImageButton). Pertanto, per determinare quando è stata scelta una categoria LinkButton nel Repeater, è necessario eseguire le operazioni seguenti:

1. Impostare il `CommandName` proprietà del controllo LinkButton nel Repeater s `ItemTemplate` su un valore (è già stato utilizzato ListProducts). Impostando questo `CommandName` valore s LinkButton `Command` evento viene generato quando si fa clic sull'elemento LinkButton.
2. Impostare il s LinkButton `CommandArgument` sul valore dell'elemento corrente s `CategoryID`.
3. Creare un gestore eventi per il controllo Repeater s `ItemCommand` evento. Nel set di eventi gestore, la `CategoryProductsDataSource` ObjectDataSource s `CategoryID` parametro il valore passato `CommandArgument`.

Le operazioni seguenti `ItemTemplate` markup per il controllo Repeater categorie implementa i passaggi 1 e 2. Si noti come `CommandArgument` valore viene assegnato l'elemento dati s `CategoryID` utilizzando la sintassi di associazione dati:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample11.aspx)]

Ogni volta che la creazione di un `ItemCommand` gestore eventi, è consigliabile verificare sempre in ingresso `CommandName` valore perché *qualsiasi* `Command` evento generato da *qualsiasi* Button, LinkButton, o ImageButton all'interno di Repeater causerà la `ItemCommand` dell'evento da generare. Anche se si dispone solo uno di tali LinkButton ora, in futuro è (o un altro sviluppatore del team) potrebbe essere aggiungere ulteriore pulsante controlli Web Repeater che, quando si fa clic, genera lo stesso `ItemCommand` gestore dell'evento. Pertanto, è s preferibile sempre accertarsi di controllare il `CommandName` proprietà e procedere con la logica a livello di codice solo se corrisponde al valore previsto.

Dopo aver verificato che passato `CommandName` ListProducts il valore è uguale, quindi assegna il gestore dell'evento di `CategoryProductsDataSource` ObjectDataSource s `CategoryID` parametro il valore passato `CommandArgument`. Questa modifica al s ObjectDataSource `SelectParameters` automaticamente provoca DataList riassociazione stesso all'origine dati, che mostra i prodotti per la categoria selezionata.


[!code-csharp[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample12.cs)]

Con queste aggiunte, questa esercitazione è stata completata. Richiedere qualche istante test in un browser. Nella figura 14 viene visualizzata la schermata durante la prima visita la pagina. Poiché una categoria deve ancora essere selezionata, viene visualizzato alcun prodotto. Facendo clic su una categoria, ad esempio, consente di visualizzare i prodotti nella categoria di prodotto in una visualizzazione a due colonne (vedere Figura 15).


[![Nessun prodotto viene visualizzati quando prima visita la pagina](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image39.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image38.png)

**Nella figura 14**: prodotti non vengono visualizzati quando prima visita la pagina ([fare clic per visualizzare l'immagine ingrandita](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image40.png))


[![Fare clic sugli elenchi di categoria di prodotti i prodotti corrispondenti a destra](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image42.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image41.png)

**Figura 15**: facendo clic sulla categoria di prodotto vengono elencati i prodotti corrispondenti a destra ([fare clic per visualizzare l'immagine ingrandita](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image43.png))


## <a name="summary"></a>Riepilogo

Come abbiamo visto in questa esercitazione e quello precedente, possono essere distribuiti in due pagine master-details report o consolidati in uno. Visualizzazione di una relazione master/dettagli in una singola pagina, tuttavia, implica alcuni problemi sulla migliore per lo schema di layout e i record dei dettagli della pagina. Nel *Master/dettaglio con Master GridView selezionabile DetailsView dettagli* esercitazione abbiamo i record di dettagli vengono visualizzati sopra i record master; in questa esercitazione è stata utilizzata tecniche CSS per avere il valore float record master per il a sinistra dei dettagli.

Con la visualizzazione master/dettagli report, è anche avuto la possibilità di esplorare come recuperare il numero di prodotti associati a ogni categoria, nonché come eseguire la logica sul lato server quando un LinkButton (pulsante o ImageButton) viene selezionato dall'interno un Repeater.

In questa esercitazione viene completato l'esame del report master/dettaglio DataList e Repeater. Il set successivo di esercitazioni viene illustrato come aggiungere, modificare ed eliminare le funzionalità del controllo DataList.

Buona programmazione!

## <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Floatutorial](http://css.maxdesign.com.au/floatutorial/) un'esercitazione sul mobile elementi CSS con CSS
- [Il posizionamento CSS](http://www.brainjar.com/css/positioning/) ulteriori informazioni sul posizionamento di elementi con CSS
- [Layout Out contenuto con HTML](http://www.w3schools.com/html/html_layout.asp) utilizzando `<table>` s e altri elementi HTML per il posizionamento

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore per questa esercitazione è stata Zack Jones. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Precedente](master-detail-filtering-acess-two-pages-datalist-cs.md)
[Successivo](master-detail-filtering-with-a-dropdownlist-datalist-vb.md)
