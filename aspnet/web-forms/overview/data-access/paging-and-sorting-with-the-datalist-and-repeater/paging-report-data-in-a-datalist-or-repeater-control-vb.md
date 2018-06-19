---
uid: web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/paging-report-data-in-a-datalist-or-repeater-control-vb
title: Spostamento di dati del Report in un controllo DataList o Repeater (VB) | Documenti Microsoft
author: rick-anderson
description: Durante il paging automatico offerta DataList né Ripetitore o il supporto dell'ordinamento, in questa esercitazione viene illustrato come aggiungere il supporto di paging per il controllo DataList o Repeater,...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2006
ms.topic: article
ms.assetid: bbd6b7f7-b98a-48b4-93f3-341d6a4f53c0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/paging-report-data-in-a-datalist-or-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 867f2a0a6de6da2ccda1526ef7c1d0edd97431c6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887317"
---
<a name="paging-report-data-in-a-datalist-or-repeater-control-vb"></a>Il paging dati di Report in un controllo DataList o Repeater (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare App di esempio](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_44_VB.exe) o [Scarica il PDF](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/datatutorial44vb1.pdf)

> Mentre DataList né Ripetitore offerta automatica spostamento o l'ordinamento di supporto, in questa esercitazione viene illustrato come aggiungere il supporto di paging in DataList o Repeater, che consente di paging e i dati molto più flessibile le interfacce di visualizzazione.


## <a name="introduction"></a>Introduzione

Ordinamento e paging sono due caratteristiche molto comune quando si visualizzano i dati in un'applicazione in linea. Ad esempio, quando si cerca documentazione ASP.NET in una libreria online, possono essere presenti centinaia di libri, ma il report che elenca i risultati della ricerca vengono elencati solo dieci corrispondenze per ogni pagina. Inoltre, i risultati possono essere ordinati per titolo, prezzo, conteggio delle pagine, il nome dell'autore e così via. Come accennato nel [Paging e ordinamento dei dati del Report](../paging-and-sorting/paging-and-sorting-report-data-vb.md) esercitazione, i controlli di GridView, DetailsView e FormView tutti fornisce supporto incorporato di paging che è possibile abilitare il segno di graduazione di una casella di controllo. Controllo GridView. include inoltre supporto per l'ordinamento.

Purtroppo, DataList né Ripetitore offrono il paging automatico o supporto per l'ordinamento. In questa esercitazione si esamineranno come aggiungere il supporto di paging per il controllo DataList o Repeater. È necessario creare l'interfaccia di paging, visualizzare la pagina appropriata del record e ricordare la pagina visitata postback a manualmente. Durante l'operazione richiedere più tempo e codice anziché con il controllo GridView, DetailsView o FormView, il DataList e Repeater consentono più flessibile di paging e dati interfacce di visualizzazione.

> [!NOTE]
> In questa esercitazione illustra esclusivamente il paging. Nella prossima esercitazione si verranno prese in esame l'aggiunta di funzionalità di ordinamento.


## <a name="step-1-adding-the-paging-and-sorting-tutorial-web-pages"></a>Passaggio 1: Aggiungere la funzionalità di Paging e l'ordinamento le pagine Web dell'esercitazione

Prima di iniziare questa esercitazione, lasciare s innanzitutto è opportuno aggiungere le pagine ASP.NET che è necessario per questa esercitazione e quello successivo. Iniziare creando una nuova cartella nel progetto denominato `PagingSortingDataListRepeater`. Successivamente, aggiungere le seguenti cinque pagine ASP.NET in questa cartella, con tutti gli elementi configurati per utilizzare la pagina master `Site.master`:

- `Default.aspx`
- `Paging.aspx`
- `Sorting.aspx`
- `SortingWithDefaultPaging.aspx`
- `SortingWithCustomPaging.aspx`


![Creare una cartella PagingSortingDataListRepeater e aggiungere le pagine ASP.NET dell'esercitazione](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image1.png)

**Figura 1**: creare un `PagingSortingDataListRepeater` cartella e aggiungere le pagine ASP.NET Tutorial


Aprire quindi il `Default.aspx` pagina e trascinare il `SectionLevelTutorialListing.ascx` controllo utente dal `UserControls` cartella nell'area di progettazione. Questo controllo utente, creata nel [pagine Master e esplorazione del sito](../introduction/master-pages-and-site-navigation-vb.md) esercitazione enumera la mappa del sito e visualizza tali esercitazioni nella sezione corrente in un elenco puntato.


[![Aggiungere il controllo utente SectionLevelTutorialListing.ascx Default.aspx](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image3.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image2.png)

**Figura 2**: aggiungere il `SectionLevelTutorialListing.ascx` controllo utente al `Default.aspx` ([fare clic per visualizzare l'immagine ingrandita](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image4.png))


Per visualizzare l'elenco puntato per lo spostamento e l'ordinamento di esercitazioni che verrà creata, è necessario aggiungerli alla mappa del sito. Aprire il `Web.sitemap` file e aggiungere il markup seguente dopo la modifica ed eliminazione con il markup di nodo DataList mappa del sito:


[!code-xml[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample1.xml)]


![Aggiornare la mappa per includere le nuove pagine ASP.NET](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image5.png)

**Figura 3**: aggiornare la mappa per includere le nuove pagine ASP.NET


## <a name="a-review-of-paging"></a>Una revisione di Paging

Nelle esercitazioni precedenti è stato illustrato come scorrere i dati nei controlli GridView, DetailsView e FormView. Questi tre controlli offrono una forma semplice di paging chiamato *paging predefinito* che può essere implementata controllando semplicemente l'opzione Abilita Paging nello smart tag controllo s. Con il paging predefinito, ogni volta che viene richiesta una pagina di dati nella prima pagina visitare o quando l'utente passa a un'altra pagina di dati di GridView, DetailsView, controllo FormView nuovamente richieste *tutti* dei dati di ObjectDataSource. Quindi gli elementi di cattura del particolare set di record da visualizzare dato l'indice della pagina richiesta e il numero di record da visualizzare per pagina. Abbiamo parlato paging predefinito in modo dettagliato nel [Paging e ordinamento dei dati del Report](../paging-and-sorting/paging-and-sorting-report-data-vb.md) esercitazione.

Poiché il paging predefinito richiede nuovamente tutti i record per ogni pagina, non è pratico quando paging sufficientemente grandi quantità di dati. Si supponga, ad esempio, paging 50.000 record con una dimensione di pagina pari a 10. Ogni volta che l'utente passa a una nuova pagina, tutte 50.000 record deve essere richiamato dal database, anche se dieci soli di essi vengono visualizzati.

*Il paging personalizzato* risolve i problemi di prestazioni del paging predefinito cattura solo il subset di record per visualizzare la pagina richiesta preciso. Quando si implementa il paging personalizzato, è necessario scrivere la query SQL che restituisce solo il set corretto di record in modo efficiente. È stato illustrato come creare una query utilizzando SQL Server 2005 s nuovo [ `ROW_NUMBER()` (parola chiave)](http://www.4guysfromrolla.com/webtech/010406-1.shtml) nuovamente il [in modo efficiente il Paging tramite quantità di dati di grandi dimensioni](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb.md) esercitazione.

Per implementare il paging predefinito nei controlli del controllo DataList o Repeater, è possibile utilizzare il [ `PagedDataSource` classe](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.aspx) come wrapper per il `ProductsDataTable` il cui contenuto è il paging. Il `PagedDataSource` classe dispone di un `DataSource` proprietà che possono essere assegnati a un oggetto enumerabile e [ `PageSize` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.pagesize.aspx) e [ `CurrentPageIndex` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.currentpageindex.aspx) le proprietà che indicano il numero di record a visualizzare ogni pagina e l'indice della pagina corrente. Una volta queste proprietà sono state impostate, il `PagedDataSource` può essere utilizzato come origine dati di qualsiasi dato controllo Web. Il `PagedDataSource`, quando enumerata, verrà restituito solo il subset appropriato di record della relativa interna `DataSource` in base il `PageSize` e `CurrentPageIndex` proprietà. Figura 4 illustra le funzionalità del `PagedDataSource` classe.


![Il PagedDataSource esegue il wrapping di un oggetto enumerabile con un'interfaccia di paging](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image6.png)

**Figura 4**: il `PagedDataSource` esegue il wrapping di un oggetto enumerabile con un'interfaccia di paging


Il `PagedDataSource` oggetto può essere creato e configurato direttamente dal livello di logica di Business e associato a un controllo DataList o Repeater tramite ObjectDataSource, o possono essere creati e configurati direttamente nella classe code-behind di pagine ASP.NET. Se viene utilizzato il secondo approccio, è necessario evitare di utilizzare ObjectDataSource e si associa i dati di paging a livello di codice per il controllo DataList o Repeater.

Il `PagedDataSource` oggetto dispone inoltre di proprietà per supportare il paging personalizzato. È tuttavia possibile ignorare utilizzando un `PagedDataSource` per il paging personalizzato perché sono già presenti i metodi di BLL `ProductsBLL` classe progettata per il paging personalizzato che restituisce i record precisi da visualizzare.

In questa esercitazione verrà esaminato l'implementazione di paging predefinito in un controllo DataList aggiungendo un nuovo metodo per il `ProductsBLL` classe che restituisce configurato in modo appropriato `PagedDataSource` oggetto. Nella prossima esercitazione, vedremo come utilizzare il paging personalizzato.

## <a name="step-2-adding-a-default-paging-method-in-the-business-logic-layer"></a>Passaggio 2: Aggiunta di un metodo di Paging predefinito nel livello di logica di Business

Il `ProductsBLL` classe dispone attualmente di un metodo per la restituzione di tutte le informazioni prodotto `GetProducts()` e uno per la restituzione di un determinato sottoinsieme di prodotti in corrispondenza dell'indice inizia `GetProductsPaged(startRowIndex, maximumRows)`. Con paging predefinito, il controllo GridView, DetailsView e FormView controlla l'uso di tutte le `GetProducts()` metodo per recuperare tutti i prodotti, ma usa un `PagedDataSource` internamente per visualizzare solo il subset corretto di record. Per replicare questa funzionalità con i controlli DataList e Repeater, è possibile creare un nuovo metodo nella BLL che simuli questo comportamento.

Aggiungere un metodo per il `ProductsBLL` classe denominata `GetProductsAsPagedDataSource` che accetta due parametri di input di interi:

- `pageIndex` l'indice della pagina per visualizzare, indicizzate da zero, e
- `pageSize` il numero di record da visualizzare per ogni pagina.

`GetProductsAsPagedDataSource` inizia recuperando *tutti* dei record da `GetProducts()`. Crea quindi un `PagedDataSource` oggetto impostando il relativo `CurrentPageIndex` e `PageSize` proprietà ai valori di passato `pageIndex` e `pageSize` parametri. Il metodo si conclude con la restituzione di questa configurazione `PagedDataSource`:


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample2.vb)]

## <a name="step-3-displaying-product-information-in-a-datalist-using-default-paging"></a>Passaggio 3: Visualizzazione di informazioni sui prodotti in un controllo DataList utilizzo del Paging predefinito

Con il `GetProductsAsPagedDataSource` aggiunto al metodo di `ProductsBLL` (classe), è ora possibile creare un controllo DataList o Repeater che fornisce il paging predefinito. Aprire il `Paging.aspx` nella pagina di `PagingSortingDataListRepeater` cartelle e trascinare un controllo DataList dalla casella degli strumenti nella finestra di progettazione, l'impostazione di DataList s `ID` proprietà `ProductsDefaultPaging`. Lo smart tag DataList s, creare un nuovo oggetto ObjectDataSource denominato `ProductsDefaultPagingDataSource` e configurarlo in modo che recuperi dati usando il `GetProductsAsPagedDataSource` metodo.


[![Creare un ObjectDataSource e configurarlo per utilizzare il metodo () GetProductsAsPagedDataSource](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image8.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image7.png)

**Figura 5**: creare un ObjectDataSource e configurarlo per utilizzare il `GetProductsAsPagedDataSource` `()` metodo ([fare clic per visualizzare l'immagine ingrandita](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image9.png))


Impostare gli elenchi a discesa nell'aggiornamento, inserimento ed eliminare le schede su (nessuno).


[![Impostare l'elenco a discesa sono elencati in UPDATE, INSERT ed eliminare le tabulazioni su (nessuno)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image11.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image10.png)

**Figura 6**: impostare l'elenco a discesa sono elencati in UPDATE, INSERT ed eliminare le tabulazioni su (nessuno) ([fare clic per visualizzare l'immagine ingrandita](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image12.png))


Poiché il `GetProductsAsPagedDataSource` metodo prevede due parametri di input, la procedura guidata richiede Microsoft per l'origine di questi valori di parametro.

L'indice della pagina e i valori di dimensioni di pagina devono essere riconosciuti durante i postback. Essi possono essere archiviati nello stato di visualizzazione, persistente per la stringa di query, archiviati nelle variabili di sessione o memorizzate utilizzando un'altra tecnica. Per questa esercitazione verrà utilizzata la stringa di query, che ha il vantaggio di consentire una particolare pagina di dati per essere rimossa.

In particolare, usare il querystring campi pageIndex e pageSize per il `pageIndex` e `pageSize` parametri, rispettivamente (vedere la figura 7). È opportuno impostare i valori predefiniti per questi parametri, come i valori di stringa di query ha vinto t essere presente quando un utente visita prima di questa pagina. Per `pageIndex`, impostare il valore predefinito 0 (che verrà visualizzata la prima pagina di dati) e `pageSize` il valore predefinito s a 4.


[![Utilizzare il parametro QueryString come origine per i parametri di pageIndex e pageSize](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image14.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image13.png)

**Figura 7**: usare il parametro QueryString come origine per il `pageIndex` e `pageSize` parametri ([fare clic per visualizzare l'immagine ingrandita](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image15.png))


Dopo aver configurato il ObjectDataSource, Visual Studio crea automaticamente un `ItemTemplate` per DataList. Personalizzare il `ItemTemplate` in modo che vengono visualizzati solo il nome del prodotto s, categoria e fornitore. Inoltre impostare DataList s `RepeatColumns` proprietà su 2, il relativo `Width` su 100% e il relativo `ItemStyle` s `Width` al 50%. Queste impostazioni di larghezza fornirà uguale spaziatura per le due colonne.

Dopo aver apportato queste modifiche, il markup s DataList e ObjectDataSource dovrebbe essere simile al seguente:


[!code-aspx[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample3.aspx)]

> [!NOTE]
> Poiché non si esegue un aggiornamento o eliminazione in questa esercitazione, è possibile disabilitare lo stato di visualizzazione DataList s per ridurre le dimensioni di pagina sottoposta a rendering.


Quando inizialmente visitare questa pagina tramite un browser, né il `pageIndex` né `pageSize` i parametri querystring vengono forniti. Di conseguenza, vengono utilizzati i valori predefiniti pari a 0 e 4. Come illustrato nella figura 8, ciò comporta un DataList che consente di visualizzare i primi quattro prodotti.


[![Sono elencati i primi quattro prodotti](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image17.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image16.png)

**Figura 8**: sono elencati il primo quattro prodotti ([fare clic per visualizzare l'immagine ingrandita](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image18.png))


Senza un'interfaccia di paging, qui s attualmente non semplice significa che un utente di spostarsi alla seconda pagina di dati. Verrà creata un'interfaccia di paging nel passaggio 4. Per il momento, tuttavia, il paging possa essere completato solo specificando direttamente i criteri di paging nella stringa di query. Ad esempio, per visualizzare la seconda pagina, modificare l'URL nella barra degli indirizzi del browser s da `Paging.aspx` a `Paging.aspx?pageIndex=2` e premere INVIO. In questo modo la seconda pagina di dati da visualizzare (vedere Figura 9).


[![Viene visualizzato la seconda pagina di dati](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image20.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image19.png)

**Figura 9**: viene visualizzato la seconda pagina di dati ([fare clic per visualizzare l'immagine ingrandita](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image21.png))


## <a name="step-4-creating-the-paging-interface"></a>Passaggio 4: Creazione dell'interfaccia di Paging

Sono disponibili diverse interfacce di paging diversi che possono essere implementate. I controlli di GridView, DetailsView e FormView forniscono quattro interfacce diverse per scegliere tra:

- **Successivo, precedente** gli utenti possono spostare una pagina alla volta, a quello successivo o precedente.
- **Successivo, precedente, primo, ultimo** oltre ai pulsanti Avanti e indietro, questa interfaccia include pulsanti First e Last per lo spostamento alla prima o ultima pagina.
- **Numerico** sono elencati i numeri di pagina nell'interfaccia di paging, consentendo all'utente di passare rapidamente a una pagina particolare.
- **Numerici, in primo luogo, ultimo** oltre i numeri di pagina numerici include pulsanti per spostarsi a una molto prima o nell'ultima pagina.

Per DataList e Repeater, ci sono responsabili per decidere su un'interfaccia di paging e implementarlo. Include la creazione di controlli Web necessari nella pagina e visualizzare la pagina richiesta quando viene premuto un pulsante di interfaccia di paging particolare. Inoltre, alcuni controlli di interfaccia di paging potrebbe essere necessario deve essere disabilitata. Ad esempio, quando si visualizza la prima pagina di dati di utilizzo successivo, precedente, primo, ultimo interfaccia, pulsanti precedente e prima sarà disabilitati.

Per questa esercitazione, s let uso successivo, precedente, prima di tutto, ultima interfaccia. Aggiungere quattro controlli pulsante Web alla pagina e impostare i relativi `ID` s per `FirstPage`, `PrevPage`, `NextPage`, e `LastPage`. Impostare il `Text` proprietà &lt; &lt; innanzitutto &lt; Prev, accanto &gt;e l'ultimo &gt; &gt; .


[!code-aspx[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample4.aspx)]

Creare quindi un `Click` gestore eventi per ognuno di questi pulsanti. In un momento in cui verrà aggiunto il codice necessario per visualizzare la pagina richiesta.

## <a name="remembering-the-total-number-of-records-being-paged-through"></a>Il numero totale di record il paging tramite memorizzazione

Indipendentemente dall'interfaccia di paging selezionata, è necessario calcolare e memorizza il numero totale di record il paging tramite. Il numero totale di righe (in combinazione con le dimensioni della pagina) determina il numero totale di pagine di dati sono il paging, che determina quali controlli dell'interfaccia di paging vengono aggiunti o sono abilitati. Nel successivo, precedente, primo, ultimo interfaccia che verrà creato, il conteggio delle pagine viene utilizzata in due modi:

- Per determinare se viene visualizzato nell'ultima pagina, nel qual caso vengono disabilitati i pulsanti Avanti e ultimo.
- Se l'utente fa clic sul pulsante ultima che dobbiamo whisk li all'ultima pagina, il cui indice è minore di pagina di conteggio.

Il conteggio delle pagine viene calcolato come il limite massimo del numero di riga del totale diviso per le dimensioni della pagina. Ad esempio, se si sta paging 79 record con quattro record per ogni pagina, quindi il conteggio delle pagine è 20 (il limite massimo di 79 / 4). Se si sta usando l'interfaccia di paging numerico, queste informazioni è indicato che il numero di pulsanti di pagina numerici da visualizzare. Se l'interfaccia di paging include pulsanti Avanti o ultimo, il conteggio delle pagine viene utilizzato per determinare quando disabilitare i pulsanti Avanti o ultimo.

Se l'interfaccia di paging include un pulsante ultima, è fondamentale che il numero totale di record il paging tramite memorizzato postback in modo che quando si fa clic sul pulsante ultima è possibile determinare l'ultimo indice della pagina. A tale scopo, creare un `TotalRowCount` la classe code-behind di pagine ASP.NET che rende persistente il valore per lo stato di visualizzazione proprietà:


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample5.vb)]

Oltre a `TotalRowCount`, richiedere un minuto, per creare le proprietà a livello di pagina di sola lettura per l'accesso con facilità l'indice della pagina, dimensioni della pagina e conteggio delle pagine:


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample6.vb)]

## <a name="determining-the-total-number-of-records-being-paged-through"></a>Determinazione del numero totale di record, il paging tramite

Il `PagedDataSource` oggetto restituito da ObjectDataSource s `Select()` metodo presenta all'interno di esso *tutti* dei record di prodotto, anche se solo un sottoinsieme di essi vengono visualizzati in DataList. Il `PagedDataSource` s [ `Count` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.count.aspx) restituisce solo il numero di elementi che verranno visualizzati in DataList; il [ `DataSourceCount` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.datasourcecount.aspx) restituisce il numero totale di elementi all'interno di `PagedDataSource`. Pertanto, è necessario assegnare ASP.NET pagina s `TotalRowCount` il valore della proprietà del `PagedDataSource` s `DataSourceCount` proprietà.

A tale scopo, creare un gestore eventi per ObjectDataSource s `Selected` evento. Nel `Selected` gestore eventi è disponibile l'accesso al valore restituito di s ObjectDataSource `Select()` metodo in questo caso, il `PagedDataSource`.


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample7.vb)]

## <a name="displaying-the-requested-page-of-data"></a>Visualizzare la pagina di dati richiesta

Quando l'utente sceglie un pulsante nell'interfaccia di paging, è necessario visualizzare la pagina richiesta dei dati. Poiché i parametri di paging vengono specificati tramite la stringa di query per visualizzare la pagina di dati utilizzare richiesta `Response.Redirect(url)` per l'utente s browser nuovamente richiedere il `Paging.aspx` pagina con i parametri appropriati di paging. Per visualizzare la seconda pagina di dati, ad esempio, si sarebbe reindirizzerà l'utente a `Paging.aspx?pageIndex=1`.

A tale scopo, creare un `RedirectUser(sendUserToPageIndex)` metodo che reindirizza l'utente a `Paging.aspx?pageIndex=sendUserToPageIndex`. Quindi, questo metodo viene chiamato dal pulsante quattro `Click` gestori eventi. Nel `FirstPage` `Click` gestore dell'evento, chiamare `RedirectUser(0)`per inviarli alla prima pagina; nel `PrevPage` `Click` gestore dell'evento, utilizzare `PageIndex - 1` come l'indice della pagina; e così via.


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample8.vb)]

Con il `Click` completare i gestori eventi, i record di DataList s possono essere trasferiti tramite facendo clic sui pulsanti. È opportuno Provalo!

## <a name="disabling-paging-interface-controls"></a>La disabilitazione di controlli dell'interfaccia di Paging

Attualmente, tutti i quattro pulsanti sono abilitati indipendentemente dalla pagina che viene visualizzato. Tuttavia, è necessario disabilitare i pulsanti del primo e precedente, quando vengono visualizzate la prima pagina di dati e i pulsanti Avanti e ultimo quando l'ultima pagina. Il `PagedDataSource` oggetto restituito da ObjectDataSource s `Select()` metodo dispone di proprietà [ `IsFirstPage` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.isfirstpage.aspx) e [ `IsLastPage` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.islastpage.aspx) che è possibile esaminare per determinare se viene visualizzato la prima o nell'ultima pagina di dati.

Aggiungere quanto segue al s ObjectDataSource `Selected` gestore eventi:


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample9.vb)]

Con l'aggiunta, i pulsanti First e precedente verranno disabilitati quando si visualizzano la prima pagina, mentre i pulsanti Avanti e ultimo verranno disabilitati quando si visualizza l'ultima pagina.

S consentono di completare l'interfaccia di paging informa l'utente la pagina è attualmente visualizzato e il numero totale di pagine esistano. Aggiungere un controllo etichetta Web alla pagina e impostare il relativo `ID` proprietà `CurrentPageNumber`. Impostare il relativo `Text` proprietà ObjectDataSource s selezionati al gestore eventi tali che include la pagina corrente visualizzata (`PageIndex + 1`) e il numero totale di pagine (`PageCount`).


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample10.vb)]

Figura 10 è illustrata `Paging.aspx` alla prima visita. Poiché la stringa di query è vuoto, il valore predefinito è DataList che mostra i primi quattro prodotti; i primo i pulsanti sono disabilitati. Fare clic su Avanti consente di visualizzare i record successivi quattro (vedere Figura 11); i primo i pulsanti sono ora abilitati.


[![Viene visualizzato la prima pagina di dati](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image23.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image22.png)

**Figura 10**: viene visualizzato la prima pagina di dati ([fare clic per visualizzare l'immagine ingrandita](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image24.png))


[![Viene visualizzato la seconda pagina di dati](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image26.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image25.png)

**Figura 11**: viene visualizzato la seconda pagina di dati ([fare clic per visualizzare l'immagine ingrandita](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image27.png))


> [!NOTE]
> È possibile migliorare ulteriormente l'interfaccia di paging, consentendo all'utente di specificare il numero di pagine per visualizzare ogni pagina. Ad esempio, un controllo DropDownList è stato possibile aggiungere opzioni di dimensioni pagina di elenco come 5, 10, 25, 50 e tutti. Dopo aver selezionato una dimensione di pagina, l'utente dovrà essere reindirizzato al `Paging.aspx?pageIndex=0&pageSize=selectedPageSize`. Lascia l'implementazione di questa funzionalità avanzata come esercizio per il lettore.


## <a name="using-custom-paging"></a>Usa il Paging personalizzato

Le pagine di DataList tramite i dati utilizzando la tecnica di paging predefinito inefficiente. Quando il paging sufficientemente grandi quantità di dati, è fondamentale utilizzato il paging personalizzato. Anche se leggermente diversa dei dettagli di implementazione, i concetti relativi all'implementazione di paging personalizzato in un controllo DataList sono lo stesso di paging predefinito. Con il paging personalizzato, utilizzare il `ProductBLL` classe s `GetProductsPaged` metodo (anziché `GetProductsAsPagedDataSource`). Come descritto nel [in modo efficiente il Paging tramite quantità di dati di grandi dimensioni](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb.md) esercitazione `GetProductsPaged` deve essere passato l'inizio riga indice e il numero massimo di righe da restituire. Questi parametri possono essere gestiti tramite il proprio come stringa di query di `pageIndex` e `pageSize` i parametri utilizzati paging predefinita.

Poiché s non esiste alcun `PagedDataSource` con il paging personalizzato, è necessario utilizzare tecniche alternative per determinare il numero totale di record il paging tramite e se è re prima o ultima pagina di dati di visualizzazione. Il `TotalNumberOfProducts()` metodo la `ProductsBLL` classe restituisce il numero totale di prodotti il paging tramite. Per determinare se viene visualizzata la prima pagina di dati, esaminare l'indice di riga iniziale se è zero, quindi viene visualizzata la prima pagina. Se l'indice di riga iniziale e il numero massimo di righe da restituire è maggiore o uguale al numero totale di record, il paging tramite viene visualizzata l'ultima pagina.

Implementare il paging personalizzato in modo più dettagliato nella prossima esercitazione verrà illustrato.

## <a name="summary"></a>Riepilogo

DataList né Ripetitore offre fuori rilevato supporto di paging in GridView, DetailsView, FormView controlli e, tale funzionalità può essere aggiunta con il minimo sforzo. È il modo più semplice per implementare il paging predefinito per eseguire il wrapping dell'intero set di prodotti all'interno di un `PagedDataSource` e poi di associare il `PagedDataSource` per il controllo DataList o Repeater. In questa esercitazione è stato aggiunto il `GetProductsAsPagedDataSource` metodo il `ProductsBLL` classe per restituire il `PagedDataSource`. Il `ProductsBLL` classe già contiene i metodi necessari per il paging personalizzato `GetProductsPaged` e `TotalNumberOfProducts`.

Insieme a recuperare lo specifico set di record da visualizzare per il paging personalizzato o tutti i record in un `PagedDataSource` per il paging predefinito, è inoltre necessario aggiungere manualmente l'interfaccia di paging. Per questa esercitazione è stato creato un successivo, precedente, primo, ultimo interfacciarsi con quattro controlli pulsante Web. Inoltre, è stato aggiunto un controllo etichetta contenente il numero di pagina corrente e il numero totale di pagine.

Nella prossima esercitazione vedremo come aggiungere il supporto dell'ordinamento le DataList Repeater. Inoltre, vedremo come creare un controllo DataList che può essere sia di paging e ordinati (con esempi di utilizzo predefinito e il paging personalizzato).

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Lead revisori per questa esercitazione sono stati Liz Shulok, Ken Pespisa e Bernadette Leigh. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](sorting-data-in-a-datalist-or-repeater-control-cs.md)
> [Successivo](sorting-data-in-a-datalist-or-repeater-control-vb.md)
