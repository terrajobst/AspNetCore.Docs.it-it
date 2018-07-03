---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb
title: Master/dettaglio con un elenco puntato di record Master e un controllo DataList di dettagli (VB) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione verranno compressi i report di due pagine master-details dell'esercitazione precedente in una singola pagina, che mostra un elenco puntato di nomi di categoria in t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2006
ms.topic: article
ms.assetid: ee20742f-6fb7-49a0-a009-058fe363aacb
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb
msc.type: authoredcontent
ms.openlocfilehash: 5b7fdcb5ba38e89b073960d6bce473c3a5aadf8b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389638"
---
<a name="masterdetail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb"></a>Master/dettaglio con un elenco puntato di record Master e un controllo DataList di dettagli (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'App di esempio](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_35_VB.exe) o [Scarica il PDF](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/datatutorial35vb1.pdf)

> In questa esercitazione verranno compressi i report di due pagine master-details dell'esercitazione precedente in una singola pagina, che mostra un elenco puntato di nomi di categorie sul lato sinistro della schermata e i prodotti della categoria selezionata a destra della schermata.


## <a name="introduction"></a>Introduzione

Nel [esercitazione precedente](master-detail-filtering-acess-two-pages-datalist-vb.md) è stato descritto come separare un report master o di dettaglio su due pagine. Nella pagina master è utilizzato un controllo Repeater per il rendering di un elenco puntato delle categorie. Ogni nome di categoria è un collegamento ipertestuale che, quando si fa clic, verrebbe take all'utente di pagina dei dettagli, in cui un controllo DataList di due colonne è stato illustrato i prodotti appartenenti alla categoria selezionata.

In questa esercitazione verranno compressi dell'esercitazione di due pagine in un'unica pagina, che mostra un elenco puntato di nomi di categorie sul lato sinistro della schermata con il nome di ogni categoria viene eseguito il rendering come un controllo LinkButton. Facendo clic sul nome della categoria LinkButton provoca un postback e associa i prodotti s categoria selezionata per un controllo DataList di due colonne a destra della schermata. Oltre a visualizzare ogni nome di categoria s, il controllo Repeater a sinistra mostra del numero totale di prodotti sono per una determinata categoria (vedere la figura 1).


[![La categoria s nome e il numero totale di prodotti vengono visualizzate a sinistra](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image2.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image1.png)

**Figura 1**: la categoria s nome e numero totale di prodotti vengono visualizzate a sinistra ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image3.png))


## <a name="step-1-displaying-a-repeater-in-the-left-portion-of-the-screen"></a>Passaggio 1: Visualizzazione di un controllo Repeater nella parte sinistra della schermata

Per questa esercitazione è necessario avere l'elenco puntato delle categorie vengono visualizzate a sinistra dei prodotti s categoria selezionata. Il contenuto all'interno di una pagina web può essere posizionato usando tag HTML standard elementi paragrafo, gli spazi unificatori, `<table>` s e così via o tramite le tecniche di propagazione foglio di stile (CSS). Tutte le esercitazioni finora hanno usato tecniche CSS per il posizionamento. Quando abbiamo creato l'interfaccia utente di navigazione nella nostra pagina master nel [pagine Master e spostamento nel sito](../introduction/master-pages-and-site-navigation-vb.md) esercitazione usassimo *posizionamento assoluto*, che indica l'offset dei pixel precise per la navigazione elenco e il contenuto principale. In alternativa, CSS può essere utilizzato per posizionare un elemento a destra o sinistra di un altro attraverso *mobile*. Possiamo nell'elenco puntato delle categorie vengono visualizzate a sinistra dei prodotti s categoria selezionata da Mobile Repeater a sinistra di DataList

Aprire il `CategoriesAndProducts.aspx` dalla pagina il `DataListRepeaterFiltering` cartella e aggiungere alla pagina un controllo Repeater e DataList. Impostare il controllo Repeater s `ID` al `Categories` e il controllo DataList s a `CategoryProducts`. Passare alla visualizzazione origine e inserire i controlli DataList e Repeater entro le proprie `<div>` elementi. Vale a dire, racchiudere il controllo Repeater all'interno di un `<div>` elemento prima e quindi di DataList nel proprio `<div>` elemento subito dopo il controllo Repeater. Il markup a questo punto dovrebbe essere simile al seguente:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample1.aspx)]

Per float Repeater a sinistra del controllo DataList, è necessario usare il `float` attributo di stile CSS, come illustrato di seguito:


[!code-html[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample2.html)]

Il `float: left;` viene spostato il primo `<div>` elemento a sinistra del secondo. Il `width` e `padding-right` impostazioni indicano il primo `<div>` s `width` e viene aggiunto quanto spaziatura interna tra il `<div>` contenuto dell'elemento s e il margine destro. Per altre informazioni su mobile elementi in CSS consultare il [Floatutorial](http://css.maxdesign.com.au/floatutorial/).

Anziché specificare l'impostazione di stile direttamente tramite il primo `<p>` elemento s' `style` dell'attributo, lasciare s invece creare una nuova classe CSS `Styles.css` denominato `FloatLeft`:


[!code-css[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample3.css)]

Quindi è possibile sostituire il `<div>` con `<div class="FloatLeft">`.

Dopo aver aggiunto la classe CSS e il markup nella configurazione di `CategoriesAndProducts.aspx` page, passare alla finestra di progettazione. Verrà visualizzato il controllo Repeater a virgola mobile a sinistra di DataList (sebbene destra ora entrambi vengono visualizzati come le caselle di grigie poiché è ve ancora consente di configurare le origini dati o modelli di).


[![Il controllo Repeater è resa mobile a sinistra di DataList](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image5.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image4.png)

**Figura 2**: il Repeater è resa mobile a sinistra di DataList ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image6.png))


## <a name="step-2-determining-the-number-of-products-for-each-category"></a>Passaggio 2: Determinare il numero di prodotti per ogni categoria

Con s Repeater e DataList che circondano markup completo, pronti per associare i dati della categoria a Repeater controllata. Tuttavia, come mostrato nell'elenco puntato delle categorie nella figura 1, oltre a ogni nome di categoria s anche dobbiamo visualizzare il numero di prodotti associate alla categoria. Per accedere a queste informazioni è possibile:

- **Determinare questa informazione dalla classe code-behind s pagina ASP.NET.** Dato un determinato *`categoryID`* è possibile determinare il numero di prodotti associati chiamando il `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` (metodo). Questo metodo restituisce un `ProductsDataTable` dell'oggetto la cui proprietà `Count` proprietà indica quanti `ProductsRow` s è presente, che corrisponde al numero di prodotti per l'oggetto specificato *`categoryID`*. È possibile creare un `ItemDataBound` gestore dell'evento per il controllo Repeater che, per ogni categoria associato al controllo Repeater, chiama il `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` (metodo) e include il relativo conteggio nell'output.
- **Aggiorna il `CategoriesDataTable` del dataset tipizzato da includere un `NumberOfProducts` colonna.** È quindi possibile aggiornare il `GetCategories()` metodo nella `CategoriesDataTable` includere queste informazioni o, in alternativa, lasciare `GetCategories()` come-è e creare un nuovo `CategoriesDataTable` metodo chiamato `GetCategoriesAndNumberOfProducts()`.

Let s esplorare entrambe queste tecniche. Il primo approccio è più semplice da implementare poiché non abbiamo t necessità di aggiornare il livello di accesso ai dati; Tuttavia, richiede ulteriori comunicazioni con il database. La chiamata ai `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` metodo nella `ItemDataBound` gestore eventi aggiunge una chiamata di database aggiuntive per ogni categoria visualizzata nel Repeater. Con questa tecnica esistono *N* + 1 database in chiamate, in cui *N* è il numero di categorie visualizzato nel Repeater. Con il secondo approccio, viene restituito il conteggio dei prodotti con informazioni su ogni categoria dal `CategoriesBLL` classe s `GetCategories()` (o `GetCategoriesAndNumberOfProducts()`) metodo, dando come risultato solo un unico round trip al database.

## <a name="determining-the-number-of-products-in-the-itemdatabound-event-handler"></a>Determinazione del numero di prodotti nel gestore dell'evento ItemDataBound

Determinazione del numero di prodotti per ogni categoria nel Repeater s `ItemDataBound` gestore dell'evento non richiede alcuna modifica per il livello di accesso ai dati esistenti. Tutte le modifiche possono essere apportate direttamente all'interno di `CategoriesAndProducts.aspx` pagina. Iniziare aggiungendo un nuovo oggetto ObjectDataSource denominato `CategoriesDataSource` tramite lo smart tag Repeater s. A questo punto, configurare il `CategoriesDataSource` ObjectDataSource in modo che recupera i relativi dati di `CategoriesBLL` classe s `GetCategories()` (metodo).


[![Configurare ObjectDataSource per usare la classe CategoriesBLL s GetCategories() (metodo)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image8.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image7.png)

**Figura 3**: configurare ObjectDataSource per usare il `CategoriesBLL` classe s `GetCategories()` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image9.png))


Ogni elemento il `Categories` Repeater deve essere selezionabile e, quando si fa clic, causare il `CategoryProducts` DataList per visualizzare i prodotti per la categoria selezionata. A questo scopo, rendendo ogni categoria di un collegamento ipertestuale, tornare a questa pagina stesso (`CategoriesAndProducts.aspx`), ma passando il `CategoryID` tramite la stringa di query, come illustrato nell'esercitazione precedente. Il vantaggio di questo approccio è che una pagina che visualizza i prodotti di una particolare categoria s può essere contrassegnato con un segnalibro e indicizzata da un motore di ricerca.

In alternativa, è possibile inviare ogni categoria di un controllo LinkButton, che è l'approccio da usare per questa esercitazione. Il LinkButton esegue il rendering nel browser utente s come collegamento ipertestuale ma, quando si fa clic, provoca un postback; durante il postback, s DataList ObjectDataSource deve essere aggiornato per visualizzare i prodotti appartenenti alla categoria selezionata. Per questa esercitazione, con un collegamento ipertestuale ha più senso rispetto all'uso di un controllo LinkButton; Tuttavia, potrebbero esserci altri scenari in cui è più vantaggioso usare un controllo LinkButton. Sebbene l'approccio hyperlink sarà ideale per questo esempio, consentire s invece esplorare usando il LinkButton. Come si vedrà, usando un controllo LinkButton implica alcuni problemi che non sarebbero altrimenti sorgere con un collegamento ipertestuale. Usando un controllo LinkButton in questa esercitazione verrà evidenziare queste sfide pertanto consentono di fornire soluzioni per questi scenari in cui si potrà usare un controllo LinkButton anziché un collegamento ipertestuale.

> [!NOTE]
> Sono invitati a ripetere questa esercitazione Usa un controllo collegamento ipertestuale o `<a>` elemento anziché il LinkButton.


Il markup seguente mostra la sintassi dichiarativa per il controllo Repeater e ObjectDataSource. Si noti che i modelli di s Repeater eseguire il rendering di un elenco puntato con ogni elemento come un controllo LinkButton:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample4.aspx)]

> [!NOTE]
> Per questa esercitazione il Repeater deve avere il proprio stato abilitato (si noti l'omissione del `EnableViewState="False"` dalla sintassi dichiarativa per il controllo Repeater s). Nel passaggio 3 verrà creato un gestore eventi per il controllo Repeater s `ItemCommand` eventi in cui verrà aggiornata di DataList s s ObjectDataSource `SelectParameters` raccolta. Il controllo Repeater s `ItemCommand`, tuttavia, ha vinto fire t Se lo stato di visualizzazione è disabilitato. Visualizzare [Stumper A una domanda ASP.NET](http://scottonwriting.net/sowblog/posts/1263.aspx) e [la propria soluzione](http://scottonwriting.net/sowBlog/posts/1268.aspx) per altre informazioni sui motivi per cui lo stato di visualizzazione deve essere abilitato per un controllo Repeater s `ItemCommand` dell'evento da generare.


Elemento LinkButton con il `ID` valore della proprietà `ViewCategory` non è relativo `Text` set di proprietà. Se avessimo appena voler visualizzare il nome della categoria, si sarebbe impostato la proprietà Text in modo dichiarativo, tramite la sintassi di associazione dati, come illustrato di seguito:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample5.aspx)]

Tuttavia, si vuole visualizzare sia il nome di categoria 1!s *e* il numero di prodotti appartenenti alla categoria selezionata. Queste informazioni possono essere recuperate da Repeater s `ItemDataBound` gestore dell'evento, eseguire una chiamata per il `ProductBLL` classe s `GetCategoriesByProductID(categoryID)` (metodo) e determinare il numero di record viene restituito in risultante `ProductsDataTable`, come il codice seguente viene illustrato:


[!code-vb[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample6.vb)]

Iniziamo assicurandosi che si ri utilizzo di un elemento di dati (un cui `ItemType` viene `Item` o `AlternatingItem`) e quindi fare riferimento il `CategoriesRow` istanza che è appena stata associata all'oggetto corrente `RepeaterItem`. Successivamente, è determinare il numero di prodotti per categoria tramite la creazione di un'istanza del `ProductsBLL` classe, chiamata relativo `GetCategoriesByProductID(categoryID)` metodo e determinare il numero di record restituiti utilizzando il `Count` proprietà. Infine, il `ViewCategory` LinkButton in ItemTemplate sia i riferimenti e il relativo `Text` è impostata su *CategoryName* (*NumberOfProductsInCategory*), dove  *NumberOfProductsInCategory* viene formattato come un numero con cifre decimali.

> [!NOTE]
> In alternativa, avremmo potuto aggiungere una *funzione di formattazione* alla classe code-behind ASP.NET pagina s che accetta una categoria s `CategoryName` e `CategoryID` valori e restituisce il `CategoryName` concatenato con il numero di i prodotti della categoria (come determinato dalla chiamata di `GetCategoriesByProductID(categoryID)` (metodo)). I risultati di tale funzione formattazione potrebbero essere assegnati in modo dichiarativo a s LinkButton sostituendo la necessità di proprietà di testo di `ItemDataBound` gestore dell'evento. Vedere le [uso di TemplateFields nel controllo GridView](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) o [formattazione di DataList e Repeater in base al momento i dati](../displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-vb.md) esercitazioni per altre informazioni sull'uso di funzioni di formattazione.


Dopo aver aggiunto questo gestore eventi, si consiglia di testare la pagina tramite un browser. Si noti come viene elencato ogni categoria in un elenco puntato, visualizzando il nome della categoria s e il numero di prodotti associate alla categoria (vedere la figura 4).


[![Ogni nome di categoria e il numero di prodotti vengono visualizzati](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image11.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image10.png)

**Figura 4**: numero di prodotti e ogni categoria s nome vengono visualizzati ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image12.png))


## <a name="updating-thecategoriesdatatableandcategoriestableadapterto-include-the-number-of-products-for-each-category"></a>L'aggiornamento di`CategoriesDataTable`e`CategoriesTableAdapter`per includere il numero di prodotti per ogni categoria

Invece di determinare il numero di prodotti per ogni categoria che s associato al controllo Repeater, è possibile semplificare questo processo modificando la `CategoriesDataTable` e `CategoriesTableAdapter` nel livello di accesso ai dati da includere queste informazioni in modo nativo. A tale scopo, è necessario aggiungere una nuova colonna a `CategoriesDataTable` per contenere il numero di prodotti associati. Per aggiungere una nuova colonna a un oggetto DataTable, aprire il DataSet tipizzato (`App_Code\DAL\Northwind.xsd`), fare clic su oggetto DataTable da modificare e fare clic su Aggiungi / colonna. Aggiungere una nuova colonna per il `CategoriesDataTable` (vedere la figura 5).


[![Aggiungere una nuova colonna per il CategoriesDataSource](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image14.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image13.png)

**Figura 5**: aggiungere una nuova colonna per il `CategoriesDataSource` ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image15.png))


Verrà aggiunta una nuova colonna denominata `Column1`, che è possibile modificare semplicemente digitando un nome diverso. Rinominare la nuova colonna `NumberOfProducts`. Successivamente, è necessario configurare questa proprietà delle colonne s. Fare clic sulla nuova colonna e passare alla finestra Proprietà. Modificare la colonna s `DataType` proprietà dal `System.String` al `System.Int32` e impostare il `ReadOnly` proprietà `True`, come illustrato nella figura 6.


![Impostare le proprietà di sola lettura della nuova colonna e il tipo di dati](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image16.png)

**Figura 6**: impostare il `DataType` e `ReadOnly` le proprietà della nuova colonna


Mentre il `CategoriesDataTable` include ora un `NumberOfProducts` colonna, il relativo valore non è impostato da una delle query s TableAdapter corrispondente. È possibile aggiornare il `GetCategories()` per restituire queste informazioni se si vuole che tali informazioni restituite ogni volta che viene recuperati informazioni sulle categorie. Se, tuttavia, è necessario solo ottenere il numero di prodotti associati per le categorie in rari casi (ad esempio solo per questa esercitazione), quindi è possibile lasciare `GetCategories()` come- e creare un nuovo metodo che restituisce queste informazioni. S ti permettono di usare questo secondo approccio, la creazione di un nuovo metodo denominato `GetCategoriesAndNumberOfProducts()`.

Per aggiungere nuovi ciò `GetCategoriesAndNumberOfProducts()` metodo, pulsante destro del mouse sul `CategoriesTableAdapter` e scegliere Nuova Query. In questo modo verticale TableAdapter Query configurazione guidata, che si va usato più volte nelle esercitazioni precedenti. Per questo metodo, avviare la procedura guidata, indicando che la query utilizza un'istruzione SQL ad hoc che vengono restituite righe.


[![Creare il metodo usando un'istruzione SQL Ad Hoc](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image18.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image17.png)

**Figura 7**: creare il metodo usando un'istruzione SQL Ad Hoc ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image19.png))


[![L'istruzione SQL restituisce righe](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image21.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image20.png)

**Figura 8**: The SQL istruzione restituisce righe ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image22.png))


La schermata successiva della procedura richiede per la query da usare. Per restituire ogni categoria 1!s `CategoryID`, `CategoryName`, e `Description` campi, insieme al numero di prodotti associati alla categoria, usano il comando seguente `SELECT` istruzione:


[!code-sql[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample7.sql)]


[![Specificare la Query da usare](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image24.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image23.png)

**Figura 9**: specificare la Query da utilizzare ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image25.png))


Si noti che la sottoquery che calcola il numero di prodotti associati con la categoria è associato un alias come `NumberOfProducts`. Questa corrispondenza dei nomi fa sì che il valore restituito da questa sottoquery può essere associato il `CategoriesDataTable` s `NumberOfProducts` colonna.

Dopo aver immesso la query, l'ultimo passaggio consiste nello scegliere il nome per il nuovo metodo. Uso `FillWithNumberOfProducts` e `GetCategoriesAndNumberOfProducts` per il riempimento un DataTable e restituire un oggetto DataTable modelli, rispettivamente.


[![Nome di metodi FillWithNumberOfProducts nuovo TableAdapter s e GetCategoriesAndNumberOfProducts](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image27.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image26.png)

**Figura 10**: assegnare un nome ai TableAdapter nuovi metodi `FillWithNumberOfProducts` e `GetCategoriesAndNumberOfProducts` ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image28.png))


A questo punto il livello di accesso ai dati è stata estesa per includere il numero di prodotti per categoria. Poiché tutti i nostri livello di presentazione consente di indirizzare tutte le chiamate alla funzione tramite un livello di logica di Business separato DAL dobbiamo aggiungere un oggetto corrispondente `GetCategoriesAndNumberOfProducts` metodo di `CategoriesBLL` classe:


[!code-vb[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample8.vb)]

Con DAL e BLL completa, è nuovamente pronto per associare dati al `Categories` Repeater in `CategoriesAndProducts.aspx`! Se è stato già creato ObjectDataSource per Repeater da di determinare il numero di prodotti nel `ItemDataBound` sezione del gestore di evento, eliminare questo ObjectDataSource e rimuovere il Repeater s `DataSourceID` proprietà impostazione; inoltre liberare dai cavi il S Repeater `ItemDataBound` evento dal gestore dell'evento rimuovendo il `Handles Categories.OnItemDataBound` sintassi nella classe code-behind ASP.NET.

Con il controllo Repeater nuovamente nello stato originale, aggiungere un nuovo oggetto ObjectDataSource denominato `CategoriesDataSource` tramite lo smart tag Repeater s. Configurare ObjectDataSource per usare la `CategoriesBLL` (classe), ma viene invece di usare il `GetCategories()` metodo, necessario utilizzare `GetCategoriesAndNumberOfProducts()` invece (vedere la figura 11).


[![Configurare ObjectDataSource per usare il metodo GetCategoriesAndNumberOfProducts](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image30.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image29.png)

**Figura 11**: configurare ObjectDataSource per usare il `GetCategoriesAndNumberOfProducts` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image31.png))


A questo punto, aggiornare il `ItemTemplate` in modo che la s LinkButton `Text` proprietà viene assegnata in modo dichiarativo utilizzando la sintassi di associazione dati e include sia la `CategoryName` e `NumberOfProducts` campi dati. Il markup dichiarativo completo per il controllo Repeater e `CategoriesDataSource` ObjectDataSource seguire:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample9.aspx)]

L'output sottoposto a rendering tramite l'aggiornamento da includere un `NumberOfProducts` colonna è simile all'utilizzo di `ItemDataBound` approccio gestore dell'evento (fare riferimento a 4 nella figura per visualizzare una schermata di cattura del ripetitore con i nomi di categoria e il numero di prodotti).

## <a name="step-3-displaying-the-selected-category-s-products"></a>Passaggio 3: Visualizzare i prodotti s categoria selezionata

A questo punto si dispone di `Categories` Repeater la visualizzazione dell'elenco di categorie oltre al numero di prodotti in ogni categoria. Il Repeater utilizza un LinkButton per ogni categoria che, quando si fa clic, comporta un postback, in corrispondenza del quale punto è necessario visualizzare tali prodotti per la categoria selezionata nel `CategoryProducts` DataList.

Una sfida che ci viene illustrato come fare in modo che il controllo DataList visualizzi solo i prodotti per la categoria selezionata. Nel [Master/dettaglio l'uso di Master GridView selezionabile con un controllo DetailsView dettagli](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md) è stato possibile selezionare l'esercitazione è stato illustrato come compilare un controllo GridView contenente le righe, con la riga selezionata s dettagli visualizzati in un controllo DetailsView nella stessa pagina. Le s GridView ObjectDataSource restituite informazioni su tutti i prodotti con il `ProductsBLL` s `GetProducts()` metodo durante il controllo DetailsView s ObjectDataSource recuperate informazioni sull'utilizzo prodotto selezionato il `GetProductsByProductID(productID)` (metodo). Il *`productID`* valore del parametro è stato specificato in modo dichiarativo mediante l'associazione con il valore di istanze della classe GridView `SelectedValue` proprietà. Non è Sfortunatamente, il controllo Repeater un `SelectedValue` proprietà e non può essere utilizzato come origine di parametro.

> [!NOTE]
> Questo è uno di tali sfide che vengono visualizzati quando si usa il LinkButton in un controllo Repeater. Se avessimo è stato usato un collegamento ipertestuale per passare il `CategoryID` tramite il parametro querystring utilizziamo, invece avremmo quel campo QueryString come origine per il valore del parametro s.


Prima ci preoccupiamo la mancanza di un `SelectedValue` proprietà per il controllo Repeater, tuttavia, consentire s prima di tutto associare ObjectDataSource DataList e come specificare relativo `ItemTemplate`.

Nello smart tag, DataList s scegliere di aggiungere un nuovo oggetto ObjectDataSource denominato `CategoryProductsDataSource` e configurarlo per usare il `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` (metodo). Poiché il controllo DataList in questa esercitazione offre un'interfaccia di sola lettura, è possibile impostare gli elenchi a discesa nell'istruzione INSERT, UPDATE, ed eliminare schede su (nessuno).


[![Configurare ObjectDataSource per utilizzare ProductsBLL s GetProductsByCategoryID(categoryID) metodo della classe](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image33.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image32.png)

**Figura 12**: configurare ObjectDataSource per utilizzare `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image34.png))


Poiché il `GetProductsByCategoryID(categoryID)` metodo prevede un parametro di input (*`categoryID`*), la procedura guidata Configura origine dati consente di specificare l'origine di s parametri. Le categorie è state elencate in un controllo DataList o GridView, d impostiamo l'elenco di riepilogo a discesa Origine parametro al controllo e il ControlID al `ID` dei dati di controllo Web. Tuttavia, poiché non ha il controllo Repeater un `SelectedValue` proprietà non può essere usato come origine di parametro. Se si archivia, si noterà che l'elenco di riepilogo a discesa ControlID contiene solo un controllo `ID``CategoryProducts`, il `ID` di DataList.

Per ora, impostare l'elenco di riepilogo a discesa Origine parametro su None. Si otterrà l'assegnazione a livello di codice il valore di questo parametro, quando una categoria LinkButton si fa clic su nel Repeater.


[![Eseguire operazioni non specifica un parametro di origine per il parametro categoryID](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image36.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image35.png)

**Figura 13**: non si specifica un parametro di origine per il *`categoryID`* parametro ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image37.png))


Dopo aver completato la procedura guidata Configura origine dati, Visual Studio genera automaticamente il controllo DataList s `ItemTemplate`. Sostituire il valore predefinito `ItemTemplate` con il modello viene usato nell'esercitazione precedente; inoltre, impostare il controllo DataList s `RepeatColumns` proprietà su 2. Dopo aver apportato queste modifiche markup dichiarativo di DataList e relativo ObjectDataSource associati dovrebbe essere simile al seguente:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample10.aspx)]

Attualmente, il `CategoryProductsDataSource` s ObjectDataSource *`categoryID`* parametro non viene impostato in modo che nessun prodotto viene visualizzato quando si visualizza la pagina. Ciò che dobbiamo fare è impostare questo valore del parametro sulla base di `CategoryID` della categoria fa clic su nel Repeater. Introduce due sfide: in primo luogo, come si determina quando un controllo LinkButton in s Repeater `ItemTemplate` è stato fatto clic; e il secondo, come è possibile determinare il `CategoryID` della categoria corrispondente il cui LinkButton selezionato?

Il LinkButton, ad esempio i controlli Button e ImageButton ha un `Click` eventi e una [ `Command` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.command.aspx). Il `Click` eventi sono progettato per semplicemente si noti che è stato fatto clic con il LinkButton. In alcuni casi, tuttavia, oltre a notare che è stato fatto clic con il LinkButton è anche necessario passare alcune informazioni aggiuntive al gestore dell'evento. In questo caso, s LinkButton [ `CommandName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.commandname.aspx) e [ `CommandArgument` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.commandargument.aspx) è possibile assegnare proprietà queste informazioni aggiuntive. Quindi, quando si seleziona il LinkButton, relativi `Command` viene generato l'evento (invece di relativi `Click` eventi) e il gestore dell'evento verrà passato i valori del `CommandName` e `CommandArgument` proprietà.

Quando un `Command` evento viene generato da all'interno di un modello nel Repeater, s Repeater [ `ItemCommand` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeater.itemcommand.aspx) viene attivato e viene passato il `CommandName` e `CommandArgument` valori del controllo LinkButton selezionato (o un pulsante o ImageButton). Pertanto, per determinare quando è stata scelta una categoria LinkButton nel Repeater, è necessario eseguire le operazioni seguenti:

1. Impostare il `CommandName` proprietà del controllo LinkButton nel Repeater s `ItemTemplate` su un valore (è già stato utilizzato ListProducts). Impostando quanto segue `CommandName` valore, la s LinkButton `Command` evento viene generato quando viene selezionato il LinkButton.
2. Impostare la s LinkButton `CommandArgument` sul valore dell'elemento corrente s `CategoryID`.
3. Creare un gestore eventi per il controllo Repeater s `ItemCommand` evento. Nel set di eventi gestore, la `CategoryProductsDataSource` s ObjectDataSource `CategoryID` parametro per il valore del passato aggiuntivo `CommandArgument`.

Nell'esempio `ItemTemplate` markup per il controllo Repeater categorie implementa i passaggi 1 e 2. Si noti come il `CommandArgument` valore viene assegnato l'elemento di dati s `CategoryID` usando la sintassi di associazione dati:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample11.aspx)]

Ogni volta che la creazione di un' `ItemCommand` gestore dell'evento è consigliabile verificare sempre prima di tutto in ingresso `CommandName` valore poiché *qualsiasi* `Command` evento generato da *qualsiasi* pulsante, LinkButton, o ImageButton all'interno di Repeater causerà il `ItemCommand` dell'evento da generare. Anche se si dispone solo una tale LinkButton a questo punto, in futuro è (o un altro sviluppatore del nostro team) Potrei aggiungere controlli pulsante aggiuntive di Web a Repeater che, quando si fa clic, genera lo stesso `ItemCommand` gestore dell'evento. Pertanto, è s preferibile sempre accertarsi di controllare il `CommandName` proprietà e procedere con la logica a livello di codice solo se stabilisce una corrispondenza tra il valore previsto.

Dopo aver verificato che passato `CommandName` valore è uguale a ListProducts, quindi assegna il gestore dell'evento il `CategoryProductsDataSource` s ObjectDataSource `CategoryID` parametro per il valore del passato aggiuntivo `CommandArgument`. Questa modifica al s ObjectDataSource `SelectParameters` automaticamente fa sì che il controllo DataList riassociare stesso all'origine dati, che mostra i prodotti per la categoria appena selezionato.


[!code-vb[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample12.vb)]

Con queste aggiunte, seguire l'esercitazione è stata completata. Si consiglia di testarlo in un browser. Figura 14 mostra la schermata durante la prima visita la pagina. Poiché una categoria deve ancora essere selezionato, non i prodotti vengono visualizzati. Facendo clic su una categoria, ad esempio produrre, consente di visualizzare i prodotti della categoria di prodotto in una visualizzazione di due colonne (vedere Figura 15).


[![Nessun prodotto viene visualizzati quando prima visita la pagina](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image39.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image38.png)

**Figura 14**: No prodotti vengono visualizzati quando prima visita la pagina ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image40.png))


[![Facendo clic sugli elenchi di categoria producono i prodotti corrispondenti a destra](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image42.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image41.png)

**Figura 15**: facendo clic sulla categoria di prodotti sono elencati i prodotti corrispondenti a destra ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image43.png))


## <a name="summary"></a>Riepilogo

Come abbiamo visto in questa esercitazione e quello precedente, master-details report possono essere distribuiti in due pagine, o consolidate in uno. Visualizzazione di una relazione master/dettaglio in una singola pagina, tuttavia, implica alcuni problemi su come meglio layout master e i record di dettagli nella pagina. Nel *Master/dettaglio l'uso di Master GridView selezionabile con un controllo DetailsView dettagli* tutorial avevamo i record di dettagli vengono visualizzati sopra i record master; in questa esercitazione è stato usato tecniche di CSS per avere il valore float record master per il sinistro dei dettagli.

Con la visualizzazione dei rapporti di dettagli/master, dovevamo anche l'opportunità di scoprire come recuperare il numero di prodotti associati a ogni categoria, nonché come eseguire la logica lato server quando un controllo LinkButton (o sul pulsante o ImageButton) si fa clic all'interno un Controllo Repeater.

Questa esercitazione si completa l'esame del report master-Details con DataList e Repeater. Il set successivo di esercitazioni verrà illustrato come aggiungere, modificare ed eliminare le funzionalità per il controllo DataList.

Buona programmazione!

## <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Floatutorial](http://css.maxdesign.com.au/floatutorial/) un'esercitazione su mobile degli elementi CSS con CSS
- [Il posizionamento CSS](http://www.brainjar.com/css/positioning/) ulteriori informazioni sul posizionamento di elementi con CSS
- [Layout orizzontale del contenuto con HTML](http://www.w3schools.com/html/html_layout.asp) usando `<table>` s e altri elementi HTML per il posizionamento

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore capo di questa esercitazione è stata Zack Jones. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](master-detail-filtering-acess-two-pages-datalist-vb.md)
