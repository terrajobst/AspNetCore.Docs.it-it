---
uid: web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/paging-report-data-in-a-datalist-or-repeater-control-vb
title: Paging di dati del Report in un controllo DataList o Repeater (VB) | Microsoft Docs
author: rick-anderson
description: Durante il DataList e Repeater il paging automatico offerta o il supporto dell'ordinamento, questa esercitazione illustra come aggiungere il supporto di paging per il controllo DataList o Repeater,...
ms.author: riande
ms.date: 11/13/2006
ms.assetid: bbd6b7f7-b98a-48b4-93f3-341d6a4f53c0
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/paging-report-data-in-a-datalist-or-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 64615f126f87cec7a96f86385ee7a717fdcdd103
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826714"
---
<a name="paging-report-data-in-a-datalist-or-repeater-control-vb"></a>Paging dei dati dei Report in un controllo DataList o Repeater (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'App di esempio](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_44_VB.exe) o [Scarica il PDF](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/datatutorial44vb1.pdf)

> Anche se nessuna delle due DataList e Repeater offerta automatica di paging o di ordinamento di supporto, questa esercitazione illustra come aggiungere il supporto di paging per il controllo DataList o Repeater, che consente visualizzazione interfacce molto più flessibile il paging e dati.


## <a name="introduction"></a>Introduzione

Paging e ordinamento sono due funzionalità molto comune quando si visualizzano i dati in un'applicazione online. Ad esempio, durante la ricerca di documentazione ASP.NET in una libreria online, possono essere presenti centinaia di questi libri, ma il report che elenca i risultati della ricerca sono elencati solo dieci corrispondenze per ogni pagina. Inoltre, i risultati possono essere ordinati per titolo, prezzo, conteggio delle pagine, il nome dell'autore e così via. Come accennato nel [Paging e ordinamento dei dati del Report](../paging-and-sorting/paging-and-sorting-report-data-vb.md) esercitazione, i controlli GridView, DetailsView e FormView tutti offrono il supporto di paging predefinito che è possibile abilitare il segno di graduazione di un controllo checkbox. Il controllo GridView include anche supporto per l'ordinamento.

Sfortunatamente, nessuna delle due DataList e Repeater offrono il paging automatico o supporto per l'ordinamento. In questa esercitazione verrà esaminato come aggiungere il supporto di paging per il controllo DataList o Repeater. Manualmente è necessario creare l'interfaccia di paging, visualizzare la pagina dei record appropriata e ricordare la pagina visitata postback. Mentre l'operazione richiedere più tempo e codice rispetto a quello con il GridView, DetailsView e FormView, DataList e Repeater consentono molto più flessibile il paging e dati interfacce di visualizzazione.

> [!NOTE]
> Questa esercitazione è incentrata esclusivamente sul paging. Nella prossima esercitazione è possibile concentrare l'attenzione all'aggiunta di funzionalità di ordinamento.


## <a name="step-1-adding-the-paging-and-sorting-tutorial-web-pages"></a>Passaggio 1: Aggiunta di Paging e ordinamento di pagine Web di esercitazione

Prima di iniziare questa esercitazione, lasciare s prima di tutto si consiglia di aggiungere le pagine ASP.NET, che sarà necessario per questa esercitazione e quello successivo. Iniziare creando una nuova cartella nel progetto denominato `PagingSortingDataListRepeater`. Successivamente, aggiungere le seguenti cinque pagine ASP.NET per questa cartella, con tutti gli elementi configurati per utilizzare la pagina master `Site.master`:

- `Default.aspx`
- `Paging.aspx`
- `Sorting.aspx`
- `SortingWithDefaultPaging.aspx`
- `SortingWithCustomPaging.aspx`


![Creare una cartella PagingSortingDataListRepeater e aggiungere le pagine ASP.NET dell'esercitazione](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image1.png)

**Figura 1**: creare un `PagingSortingDataListRepeater` cartella e aggiungere le pagine ASP.NET dell'esercitazione


Successivamente, aprire il `Default.aspx` pagina e trascinare il `SectionLevelTutorialListing.ascx` controllo utente dal `UserControls` cartella nell'area di progettazione. Questo controllo utente, che è stato creato nel [pagine Master e spostamento nel sito](../introduction/master-pages-and-site-navigation-vb.md) esercitazione enumera la mappa del sito e consente di visualizzare tali esercitazioni nella sezione corrente in un elenco puntato.


[![Aggiungere il controllo utente sectionleveltutoriallisting. ascx a default. aspx](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image3.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image2.png)

**Figura 2**: aggiungere i `SectionLevelTutorialListing.ascx` controllo utente da `Default.aspx` ([fare clic per visualizzare l'immagine con dimensioni normali](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image4.png))


Per visualizzare il paging e ordinamento esercitazioni che verrà creato in elenco puntato, è necessario aggiungerli alla mappa del sito. Aprire il `Web.sitemap` file e aggiungere il markup seguente dopo la modifica ed eliminazione in corso con il markup di nodo DataList mappa del sito:


[!code-xml[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample1.xml)]


![Aggiornare la mappa del sito in modo da includere le nuove pagine ASP.NET](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image5.png)

**Figura 3**: aggiornare la mappa del sito in modo da includere le nuove pagine ASP.NET


## <a name="a-review-of-paging"></a>Una revisione di Paging

Nelle esercitazioni precedenti è stato illustrato come spostarsi tra i dati nei controlli GridView, DetailsView e FormView. Questi tre controlli offrono una forma semplice di paging chiamato *paging predefinito* che può essere implementata da semplicemente selezionando l'opzione attiva Paging nello smart tag di controllo del codice s. Con il paging predefinito, ogni volta che viene richiesta una pagina di dati nella prima pagina visitare o quando l'utente passa a un'altra pagina di dati di GridView, DetailsView, o controllo FormView richiede nuovamente *tutti* dei dati dal ObjectDataSource. Quindi frammenti out particolare set di record da visualizzare dato indice pagina richiesta e il numero di record da visualizzare per ogni pagina. È stato illustrato il paging predefinito in modo dettagliato nel [Paging e ordinamento dei dati del Report](../paging-and-sorting/paging-and-sorting-report-data-vb.md) esercitazione.

Poiché il paging predefinito richiede nuovamente tutti i record per ogni pagina, non è pratico quando lo scorrimento sufficientemente grandi quantità di dati. Si supponga, ad esempio, paging 50.000 record con dimensioni della pagina pari a 10. Ogni volta che l'utente passa a una nuova pagina, tutte le 50.000 record devono essere recuperati dal database, anche se vengono visualizzati dieci soli di essi.

*Il paging personalizzato* consente di risolvere i problemi di prestazioni del paging predefinito utilizzando solo il subset di record da visualizzare sulla pagina richiesta preciso. Quando si implementa il paging personalizzato, è necessario scrivere la query SQL che verrà restituito in modo efficiente solo il set corretto di record. È stato illustrato come creare una query usando SQL Server 2005 s nuove [ `ROW_NUMBER()` parola chiave](http://www.4guysfromrolla.com/webtech/010406-1.shtml) nuovamente nel [in modo efficiente il Paging attraverso quantità di dati di grandi dimensioni](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb.md) esercitazione.

Per implementare il paging predefinito nei controlli DataList o Repeater, è possibile usare la [ `PagedDataSource` classe](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.aspx) come wrapper per il `ProductsDataTable` il cui contenuto è il paging. Il `PagedDataSource` classe ha un `DataSource` proprietà che possono essere assegnati a qualsiasi oggetto enumerabile e [ `PageSize` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.pagesize.aspx) e [ `CurrentPageIndex` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.currentpageindex.aspx) proprietà che indicano il numero di record a Mostra per ogni pagina e l'indice della pagina corrente. Una volta queste proprietà sono state impostate, il `PagedDataSource` può essere utilizzato come origine dati di qualsiasi controllo Web per dati. Il `PagedDataSource`, quando enumerata, verrà restituito solo subset di record della relativa interno appropriato `DataSource` in base il `PageSize` e `CurrentPageIndex` proprietà. Figura 4 viene illustrata la funzionalità del `PagedDataSource` classe.


![Il PagedDataSource esegue il wrapping di un oggetto enumerabile con un'interfaccia paginabile](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image6.png)

**Figura 4**: il `PagedDataSource` esegue il wrapping di un oggetto enumerabile con un'interfaccia paginabile


Il `PagedDataSource` oggetto può essere creato e configurato direttamente dal livello della logica di Business e associato a un controllo DataList o Repeater tramite ObjectDataSource, o può essere creato e configurato direttamente nella classe code-behind ASP.NET pagina s. Se viene utilizzato il secondo approccio, è necessario evitare di utilizzare ObjectDataSource e associare invece i dati di paging per il controllo DataList o Repeater a livello di codice.

Il `PagedDataSource` oggetto inoltre dispone di proprietà per supportare il paging personalizzato. Tuttavia, è possibile ignorare usando un `PagedDataSource` per il paging personalizzato perché abbiamo già metodi BLL `ProductsBLL` classe progettata per il paging personalizzato che restituisce i record precisi da visualizzare.

In questa esercitazione verrà esaminato implementare il paging predefinito in un controllo DataList aggiungendo un nuovo metodo per la `ProductsBLL` classe che restituisce un configurato in modo appropriato `PagedDataSource` oggetto. Nella prossima esercitazione, vedremo come usare il paging personalizzato.

## <a name="step-2-adding-a-default-paging-method-in-the-business-logic-layer"></a>Passaggio 2: Aggiunta di un metodo di Paging predefinito nel livello di logica di Business

Il `ProductsBLL` classe dispone attualmente di un metodo per la restituzione di tutte le informazioni sul prodotto `GetProducts()` e uno per la restituzione di un sottoinsieme particolare di prodotti in corrispondenza dell'indice inizio `GetProductsPaged(startRowIndex, maximumRows)`. Con il paging predefinito, il controllo GridView, DetailsView e FormView controlla l'utilizzo di tutti i `GetProducts()` metodo per recuperare tutti i prodotti, ma userà quindi una `PagedDataSource` internamente da visualizzare solo il subset corretto di record. Per replicare questa funzionalità con i controlli DataList e Repeater, è possibile creare un nuovo metodo in grado di simulare questo comportamento, il livello BLL.

Aggiungere un metodo per la `ProductsBLL` classe denominata `GetProductsAsPagedDataSource` che accetta due parametri di input di tipo integer:

- `pageIndex` l'indice della pagina da visualizzare, indicizzati da zero, e
- `pageSize` il numero di record da visualizzare per ogni pagina.

`GetProductsAsPagedDataSource` inizia recuperando *tutte* dei record da `GetProducts()`. Viene quindi creato un `PagedDataSource` oggetto, l'impostazione relativa `CurrentPageIndex` e `PageSize` delle proprietà ai valori del passato `pageIndex` e `pageSize` parametri. Il metodo termina restituendo questa configurazione `PagedDataSource`:


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample2.vb)]

## <a name="step-3-displaying-product-information-in-a-datalist-using-default-paging"></a>Passaggio 3: Visualizzare le informazioni sul prodotto in un controllo DataList di utilizzo del Paging predefinito

Con il `GetProductsAsPagedDataSource` aggiunto al metodo il `ProductsBLL` (classe), possiamo creare un controllo DataList o Repeater che fornisce il paging predefinito. Iniziare aprendo il `Paging.aspx` nella pagina la `PagingSortingDataListRepeater` cartelle e trascinare un controllo DataList dalla casella degli strumenti nella finestra di progettazione, l'impostazione di DataList s `ID` proprietà `ProductsDefaultPaging`. Nello smart tag DataList s, creare un nuovo oggetto ObjectDataSource denominato `ProductsDefaultPagingDataSource` e configurarlo in modo che recupera i dati utilizzando il `GetProductsAsPagedDataSource` (metodo).


[![Creare un oggetto ObjectDataSource e configurarlo per usare il metodo () GetProductsAsPagedDataSource](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image8.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image7.png)

**Figura 5**: creare un ObjectDataSource e configurarlo per usare il `GetProductsAsPagedDataSource` `()` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image9.png))


Impostare gli elenchi a discesa nell'aggiornamento, inserimento ed eliminare schede su (nessuno).


[![Impostare l'elenco a discesa sono elencati nell'aggiornamento, inserimento ed eliminare schede su (nessuno)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image11.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image10.png)

**Figura 6**: impostare l'elenco a discesa sono elencati nell'aggiornamento, inserimento ed eliminare schede su (nessuno) ([fare clic per visualizzare l'immagine con dimensioni normali](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image12.png))


Poiché il `GetProductsAsPagedDataSource` metodo prevede due parametri di input, la procedura guidata richiede per l'origine di questi valori di parametro.

L'indice della pagina e i valori di dimensioni di pagina devono essere memorizzati nella navigazione tra i postback. Essi possono essere archiviati nello stato di visualizzazione, persistente per la stringa di query, archiviati nelle variabili di sessione o memorizzata con un'altra tecnica. Per questa esercitazione si userà la stringa di query, che offre il vantaggio di una determinata pagina di dati a essere contrassegnato con un segnalibro.

In particolare, usare il pageIndex campi querystring e pageSize per il `pageIndex` e `pageSize` parametri, rispettivamente (vedere la figura 7). Si consiglia di impostare i valori predefiniti per questi parametri, come i valori di stringa di query ha vinto t essere presente quando un utente prima visita questa pagina. Per la `pageIndex`, impostare il valore predefinito 0 (che verrà visualizzata la prima pagina di dati) e `pageSize` valore predefinito di s a 4.


[![Utilizzare il parametro QueryString come origine per i parametri pageIndex e pageSize](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image14.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image13.png)

**Figura 7**: usare il parametro QueryString come origine per il `pageIndex` e `pageSize` parametri ([fare clic per visualizzare l'immagine con dimensioni normali](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image15.png))


Dopo la configurazione di ObjectDataSource, Visual Studio crea automaticamente un `ItemTemplate` per DataList. Personalizzare il `ItemTemplate` in modo che vengano visualizzati solo il nome del prodotto s, categoria e fornitore. Impostare anche il controllo DataList s `RepeatColumns` proprietà su 2, relativi `Width` al 100% e i relativi `ItemStyle` s `Width` al 50%. Queste impostazioni di larghezza fornirà uguale spaziatura per le due colonne.

Dopo aver apportato queste modifiche, il markup s DataList e ObjectDataSource dovrebbe essere simile al seguente:


[!code-aspx[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample3.aspx)]

> [!NOTE]
> Poiché abbiamo non stanno eseguendo eventuali aggiornamenti o funzionalità di eliminazione in questa esercitazione, è possibile disabilitare lo stato di visualizzazione DataList s per ridurre le dimensioni di pagina sottoposta a rendering.


Inizialmente gli utenti in visita questa pagina tramite un browser, né il `pageIndex` né `pageSize` ai parametri querystring vengono forniti. Di conseguenza, vengono utilizzati i valori predefiniti pari a 0 e 4. Come illustrato nella figura 8, ciò comporta un controllo DataList che visualizza i primi quattro prodotti.


[![Sono elencati i primi quattro prodotti](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image17.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image16.png)

**Figura 8**: il primo di quattro prodotti sono elencati ([fare clic per visualizzare l'immagine con dimensioni normali](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image18.png))


Senza un'interfaccia di paging, qui s attualmente non semplice significa che un utente può passare alla seconda pagina di dati. Si creerà un'interfaccia di paging nel passaggio 4. Per il momento, tuttavia, il paging solo scopo, specificare direttamente i criteri di paging nella stringa di query. Ad esempio, per visualizzare la seconda pagina, modificare l'URL nella barra degli indirizzi del browser s dalla `Paging.aspx` a `Paging.aspx?pageIndex=2` e premere INVIO. In questo modo la seconda pagina di dati da visualizzare (vedere la figura 9).


[![Viene visualizzato la seconda pagina di dati](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image20.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image19.png)

**Figura 9**: viene visualizzato la seconda pagina di dati ([fare clic per visualizzare l'immagine con dimensioni normali](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image21.png))


## <a name="step-4-creating-the-paging-interface"></a>Passaggio 4: Creazione dell'interfaccia di Paging

Sono disponibili svariate interfacce paging diverse che possono essere implementate. I controlli GridView, DetailsView e FormView forniscono quattro diverse interfacce di scegliere tra:

- **Next, Previous** gli utenti possono spostare una pagina alla volta, a quello precedente o successiva.
- **Successivo, precedente, primo, ultimo** oltre ai pulsanti Avanti e indietro, questa interfaccia include pulsanti prima e ultima per lo spostamento alla prima o ultima pagina.
- **Numerico** Elenca i numeri di pagina nell'interfaccia di paging, consentendo all'utente di passare rapidamente a una pagina particolare.
- **Numerico, in primo luogo, ultimo** oltre i numeri di pagina numerico, include i pulsanti per lo spostamento alla prima o ultima pagina.

Per con DataList e Repeater, siamo responsabile della scelta al momento di un'interfaccia di paging e implementarlo. Ciò comporta la creazione di controlli Web necessari nella pagina e visualizzare la pagina richiesta quando viene fatto clic su un particolare pulsante di interfaccia di paging. Inoltre, determinati controlli di interfaccia di paging potrebbe essere necessario deve essere disabilitata. Ad esempio, quando si visualizzano la prima pagina di dati tramite successivo, precedente, prima di tutto, ultima interfaccia, pulsanti Indietro sia il primo verrebero disabilitati.

Per questa esercitazione, usare "Let" s successivo, precedente, prima di tutto, ultima interfaccia. Aggiungere quattro controlli pulsante Web alla pagina e impostare loro `ID` s al `FirstPage`, `PrevPage`, `NextPage`, e `LastPage`. Impostare il `Text` delle proprietà per &lt; &lt; prima di tutto &lt; Prev, accanto &gt;e l'ultimo &gt; &gt; .


[!code-aspx[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample4.aspx)]

Creare quindi un `Click` gestore eventi per ognuno di questi pulsanti. Più avanti si aggiungerà il codice necessario per visualizzare la pagina richiesta.

## <a name="remembering-the-total-number-of-records-being-paged-through"></a>Ricordare il numero totale di record in fase di paging tramite

Indipendentemente dall'interfaccia di paging selezionata, è necessario calcolare e ricordare il numero totale di record in fase di paging tramite. Il numero totale di righe (in combinazione con le dimensioni della pagina) determina il numero totale di pagine di dati sono in fase di paging, che determina quali controlli di interfaccia di paging vengono aggiunti o sono abilitati. Nel successivo, precedente, primo, ultimo interfaccia che stiamo creando, il conteggio delle pagine viene usato in due modi:

- Per determinare se si sta visualizzando l'ultima pagina, nel qual caso vengono disabilitati i pulsanti Avanti e Last.
- Se l'utente fa clic sul pulsante ultima che dobbiamo whisk li all'ultima pagina, il cui indice è una minore di pagina di conteggio.

Il conteggio delle pagine viene calcolato come il limite massimo del numero di riga del totale diviso per le dimensioni della pagina. Ad esempio, se si sta lo scorrimento dei 79 record con quattro record per ogni pagina, quindi il conteggio delle pagine è 20 (il limite massimo di 79 / 4). Se si sta usando l'interfaccia di paging numerica, questa informazione è indicato per quanto riguarda il numero di pulsanti numerici da visualizzare. Se l'interfaccia di paging include i pulsanti Avanti o Last, il conteggio delle pagine viene utilizzato per determinare se disabilitare i pulsanti Avanti o ultimo.

Se l'interfaccia di paging include un pulsante per ultimo, è fondamentale che il numero totale di record in fase di paging tramite mantenuta durante i postback in modo che quando si fa clic sul pulsante ultima è possibile determinare l'ultimo indice di pagina. A tale scopo, creare un `TotalRowCount` proprietà nella classe code-behind ASP.NET pagina s che rende persistente il valore per lo stato di visualizzazione:


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample5.vb)]

Oltre a `TotalRowCount`, è opportuno creare proprietà a livello di pagina di sola lettura per accedere facilmente l'indice della pagina, le dimensioni di pagina e del conteggio delle pagine:


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample6.vb)]

## <a name="determining-the-total-number-of-records-being-paged-through"></a>Determinazione del numero totale di record in fase di paging tramite

Il `PagedDataSource` oggetto restituito da ObjectDataSource s `Select()` metodo presenta all'interno di esso *tutte* dei record di prodotto, anche se solo un sottoinsieme di essi vengono visualizzati in DataList. Il `PagedDataSource` s [ `Count` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.count.aspx) restituisce solo il numero di elementi che verrà visualizzato in DataList; la [ `DataSourceCount` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.datasourcecount.aspx) restituisce il numero totale di elementi all'interno di `PagedDataSource`. Pertanto, è necessario assegnare ASP.NET pagina s `TotalRowCount` il valore della proprietà del `PagedDataSource` s `DataSourceCount` proprietà.

A questo scopo, creare un gestore eventi per gli oggetti ObjectDataSource `Selected` evento. Nel `Selected` gestore dell'evento è possibile accedere al valore restituito di istanze della classe ObjectDataSource `Select()` metodo in questo caso, il `PagedDataSource`.


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample7.vb)]

## <a name="displaying-the-requested-page-of-data"></a>Visualizzare la pagina richiesta dei dati

Quando l'utente fa clic su uno dei pulsanti nell'interfaccia di paging, è necessario visualizzare la pagina richiesta dei dati. Poiché i parametri di paging vengono specificati tramite la stringa di query, per visualizzare la pagina richiesta di dati, utilizzare `Response.Redirect(url)` affinché l'utente s browser nuovamente richieste le `Paging.aspx` pagina con i parametri appropriati di paging. Per visualizzare la seconda pagina di dati, ad esempio, si sarebbe reindirizzare l'utente `Paging.aspx?pageIndex=1`.

A tale scopo, creare un `RedirectUser(sendUserToPageIndex)` metodo che reindirizza l'utente al `Paging.aspx?pageIndex=sendUserToPageIndex`. Quindi, chiamare questo metodo dal pulsante quattro `Click` gestori eventi. Nel `FirstPage` `Click` gestore dell'evento, chiamare `RedirectUser(0)`, di inviarle alla prima pagina; nel `PrevPage` `Click` gestore eventi, usare `PageIndex - 1` come l'indice della pagina; e così via.


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample8.vb)]

Con la `Click` completare i gestori eventi, i record di DataList s possono essere trasferiti tramite facendo clic sui pulsanti. Si consiglia di provarlo.

## <a name="disabling-paging-interface-controls"></a>La disabilitazione di controlli dell'interfaccia di Paging

Attualmente, tutti i quattro pulsanti sono abilitati indipendentemente dalla pagina visualizzata. Tuttavia, è necessario disabilitare i pulsanti del primo e precedente quando viene visualizzata la prima pagina di dati e i pulsanti Avanti e ultimo quando viene visualizzata l'ultima pagina. Il `PagedDataSource` oggetto restituito da ObjectDataSource s `Select()` metodo dispone di proprietà [ `IsFirstPage` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.isfirstpage.aspx) e [ `IsLastPage` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.islastpage.aspx) che possa essere esaminato per determinare se si sta visualizzando la prima o nell'ultima pagina di dati.

Aggiungere il codice seguente al s ObjectDataSource `Selected` gestore dell'evento:


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample9.vb)]

Grazie a questa aggiunta, i pulsanti Indietro e First verranno disabilitati quando si visualizzano la prima pagina, mentre i pulsanti Avanti e ultima verranno disabilitati quando si visualizza l'ultima pagina.

Let s completare l'interfaccia di paging per informare gli utenti sul paging sono re attualmente visualizzato e il numero totale di pagine esistano. Aggiungere un controllo etichetta Web alla pagina e impostare relativi `ID` proprietà `CurrentPageNumber`. Impostare relativi `Text` proprietà ObjectDataSource s selezionati gestore eventi questo tipo che include la pagina corrente che viene visualizzato (`PageIndex + 1`) e il numero totale di pagine (`PageCount`).


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample10.vb)]

Illustrato nella figura 10 `Paging.aspx` alla prima visita. Poiché la stringa di query è vuoto, DataList per impostazione predefinita che mostra i primi quattro prodotti; i pulsanti Indietro e First sono disabilitati. Selezione di Avanti vengono visualizzati i quattro record (vedere la figura 11); i primo i pulsanti sono ora abilitati.


[![Viene visualizzato la prima pagina di dati](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image23.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image22.png)

**Figura 10**: viene visualizzato la prima pagina di dati ([fare clic per visualizzare l'immagine con dimensioni normali](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image24.png))


[![Viene visualizzato la seconda pagina di dati](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image26.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image25.png)

**Figura 11**: viene visualizzato la seconda pagina di dati ([fare clic per visualizzare l'immagine con dimensioni normali](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image27.png))


> [!NOTE]
> L'interfaccia di paging può essere ulteriormente migliorato, consentendo all'utente di specificare il numero di pagine per visualizzare ogni pagina. Ad esempio, un controllo DropDownList è stato possibile aggiungere opzioni relative alle dimensioni di pagina di elenco, ad esempio 5, 10, 25, 50 e tutti. Dopo aver selezionato una dimensione di pagina, l'utente dovrà essere reindirizzato al `Paging.aspx?pageIndex=0&pageSize=selectedPageSize`. Lascio che implementa questa funzionalità avanzata come esercizio per il lettore.


## <a name="using-custom-paging"></a>Utilizzo del Paging personalizzato

Le pagine di DataList tramite i relativi dati tramite la tecnica di paging predefinito inefficiente. Quando il paging attraverso sufficientemente grandi quantità di dati, è fondamentale che essere utilizzato il paging personalizzato. Anche se i dettagli di implementazione è leggermente diverse, i concetti di base che implementa il paging personalizzato in un controllo DataList sono le stesse usate con il paging predefinito. Con paging personalizzato, usare il `ProductBLL` classe s `GetProductsPaged` metodo (anziché `GetProductsAsPagedDataSource`). Come descritto nel [in modo efficiente il Paging attraverso quantità di dati di grandi dimensioni](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb.md) esercitazione `GetProductsPaged` deve essere passato l'inizio riga dell'indice e il numero massimo di righe da restituire. Questi parametri possono essere gestiti tramite simili querystring semplicemente il `pageIndex` e `pageSize` parametri utilizzati parola chiave default di paging.

Poiché ci s alcun `PagedDataSource` con paging personalizzato, è necessario utilizzare tecniche alternative per determinare il numero totale di record in fase di paging tramite e se viene nuovamente visualizzato prima o ultima pagina di dati. Il `TotalNumberOfProducts()` metodo di `ProductsBLL` classe restituisce il numero totale di prodotti in fase di paging tramite. Per determinare se viene visualizzata la prima pagina di dati, esaminare l'indice di riga iniziale se è zero, quindi viene visualizzata la prima pagina. L'ultima pagina viene visualizzato se l'indice di riga iniziale e il numero massimo di righe da restituire è maggiore o uguale al numero totale di record in fase di paging tramite.

Esploreremo implementare il paging personalizzato in modo più dettagliato nella prossima esercitazione.

## <a name="summary"></a>Riepilogo

Anche se nessuna delle due DataList e Repeater offre il fuori il supporto di paging casella trovato in GridView, DetailsView, FormView controlli e, di tali funzionalità possono essere aggiunti con il minimo sforzo. Il modo più semplice per implementare il paging predefinito è eseguire il wrapping dell'intero set di prodotti all'interno di un `PagedDataSource` e quindi associano le `PagedDataSource` per il controllo DataList o Repeater. In questa esercitazione è stata aggiunta la `GetProductsAsPagedDataSource` metodo per il `ProductsBLL` classe per restituire il `PagedDataSource`. Il `ProductsBLL` già contiene i metodi necessari per il paging personalizzato `GetProductsPaged` e `TotalNumberOfProducts`.

Insieme a recuperare lo specifico set di record da visualizzare per il paging personalizzato né di tutti i record in un `PagedDataSource` per il paging predefinito, è anche necessario aggiungere manualmente l'interfaccia di paging. Per questa esercitazione è stato creato un successivo, precedente, primo, ultimo interfacciarsi con quattro controlli pulsante Web. Inoltre, è stato aggiunto un controllo etichetta che visualizza il numero di pagina corrente e il numero totale di pagine.

Nella prossima esercitazione vedremo come aggiungere il supporto di ordinamento con DataList e Repeater. Si vedrà anche come creare un controllo DataList che può essere sia di paging e ordinamento (con esempi di uso predefinito e il paging personalizzato).

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. I revisori per questa esercitazione sono Liz Shulok, Ken Pespisa e Bernadette Leigh. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](sorting-data-in-a-datalist-or-repeater-control-cs.md)
> [Successivo](sorting-data-in-a-datalist-or-repeater-control-vb.md)
