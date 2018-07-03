---
uid: web-forms/overview/data-access/paging-and-sorting/paging-and-sorting-report-data-cs
title: Paging e ordinamento del Report dei dati (c#) | Microsoft Docs
author: rick-anderson
description: Paging e ordinamento sono due funzionalità molto comune quando si visualizzano i dati in un'applicazione online. In questa esercitazione prenderemo in una prima occhiata di aggiunta di ordinamento e...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2006
ms.topic: article
ms.assetid: 811a6ef2-ec66-4c8e-a089-6f795056e288
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/paging-and-sorting-report-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 75a2206bf3db3af8859fe4de58f67135d31bba0f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37401922"
---
<a name="paging-and-sorting-report-data-c"></a>Paging e ordinamento dei dati del Report (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'App di esempio](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_24_CS.exe) o [Scarica il PDF](paging-and-sorting-report-data-cs/_static/datatutorial24cs1.pdf)

> Paging e ordinamento sono due funzionalità molto comune quando si visualizzano i dati in un'applicazione online. In questa esercitazione articolo verrà fornita una panoramica dell'aggiunta di ordinamento e paging ai nostri report, che verrà quindi creata al momento delle esercitazioni in futuro.


## <a name="introduction"></a>Introduzione

Paging e ordinamento sono due funzionalità molto comune quando si visualizzano i dati in un'applicazione online. Ad esempio, durante la ricerca di documentazione ASP.NET in una libreria online, possono essere presenti centinaia di questi libri, ma il report che elenca i risultati della ricerca sono elencati solo dieci corrispondenze per ogni pagina. Inoltre, i risultati possono essere ordinati per titolo, prezzo, conteggio delle pagine, il nome dell'autore e così via. Mentre passato 23 esercitazioni hanno esaminato come creare una varietà di report, incluse le interfacce che consentono l'aggiunta, modifica ed eliminazione di dati, abbiamo ve non preso in esame la modalità di ordinamento data e l'unico esempi di paging è ve visto hanno lavorato con il controllo DetailsView e FormView controlli.

In questa esercitazione si noterà come aggiungere l'ordinamento e paging ai nostri report, che può essere eseguito selezionando semplicemente alcune caselle di controllo. Sfortunatamente, questa implementazione semplicistica diminuisce ha relativi svantaggi che lascia qualche desiderio irrisolto l'interfaccia di ordinamento e le routine di paging non sono progettate per spostarsi in modo efficiente tra il set di risultati di grandi dimensioni. Utili per superare le limitazioni di out-of-the-box paging e ordinamento soluzioni verranno esaminati nelle esercitazioni future.

## <a name="step-1-adding-the-paging-and-sorting-tutorial-web-pages"></a>Passaggio 1: Aggiunta di Paging e ordinamento di pagine Web di esercitazione

Prima di iniziare questa esercitazione, lasciare s prima di tutto si consiglia di aggiungere le pagine ASP.NET, che sarà necessario per questa esercitazione e i successivi tre. Iniziare creando una nuova cartella nel progetto denominato `PagingAndSorting`. Successivamente, aggiungere le seguenti cinque pagine ASP.NET per questa cartella, con tutti gli elementi configurati per utilizzare la pagina master `Site.master`:

- `Default.aspx`
- `SimplePagingSorting.aspx`
- `EfficientPaging.aspx`
- `SortParameter.aspx`
- `CustomSortingUI.aspx`


![Creare una cartella PagingAndSorting e aggiungere le pagine ASP.NET dell'esercitazione](paging-and-sorting-report-data-cs/_static/image1.png)

**Figura 1**: creare una cartella PagingAndSorting e aggiungere le pagine ASP.NET dell'esercitazione


Successivamente, aprire il `Default.aspx` pagina e trascinare il `SectionLevelTutorialListing.ascx` controllo utente dal `UserControls` cartella nell'area di progettazione. Questo controllo utente, che è stato creato nel [pagine Master e spostamento nel sito](../introduction/master-pages-and-site-navigation-cs.md) esercitazione enumera la mappa del sito e consente di visualizzare tali esercitazioni nella sezione corrente in un elenco puntato.


![Aggiungere il controllo utente sectionleveltutoriallisting. ascx a default. aspx](paging-and-sorting-report-data-cs/_static/image2.png)

**Figura 2**: aggiungere il controllo utente sectionleveltutoriallisting. ascx a default. aspx


Per visualizzare il paging e ordinamento esercitazioni che verrà creato in elenco puntato, è necessario aggiungerli alla mappa del sito. Aprire il `Web.sitemap` file e aggiungere il markup seguente dopo che il markup di nodo della mappa di sito modifica, inserimento ed eliminazione:


[!code-xml[Main](paging-and-sorting-report-data-cs/samples/sample1.xml)]


![Aggiornare la mappa del sito in modo da includere le nuove pagine ASP.NET](paging-and-sorting-report-data-cs/_static/image3.png)

**Figura 3**: aggiornare la mappa del sito in modo da includere le nuove pagine ASP.NET


## <a name="step-2-displaying-product-information-in-a-gridview"></a>Passaggio 2: Visualizzare le informazioni sul prodotto in un controllo GridView

Prima che abbiamo effettivamente implementare funzionalità di ordinamento e paging, consentire s prima di tutto creare un standard non-srotable, non paginabile GridView che elenca le informazioni sul prodotto. Si tratta di un'attività si va eseguita molte volte in tutta questa serie di esercitazioni, pertanto questi passaggi dovrebbe essere familiare. Iniziare aprendo il `SimplePagingSorting.aspx` pagina e trascinare un controllo GridView dalla casella degli strumenti nella finestra di progettazione, l'impostazione relativa `ID` proprietà `Products`. Successivamente, creare un nuovo oggetto ObjectDataSource che utilizza la classe ProductsBLL s `GetProducts()` per restituire tutte le informazioni sul prodotto.


![Recuperare informazioni su tutti i prodotti usando il metodo GetProducts()](paging-and-sorting-report-data-cs/_static/image4.png)

**Figura 4**: recuperare informazioni su tutti i prodotti usando il metodo GetProducts()


Poiché questo report è un report di sola lettura, non vi s non è necessario eseguire il mapping s ObjectDataSource `Insert()`, `Update()`, o `Delete()` metodi corrispondente `ProductsBLL` metodi; pertanto, scegliere (nessuno) nell'elenco a discesa per l'aggiornamento, inserimento, e le schede di eliminazione.


![Scegliere (nessuno) opzione nell'elenco a discesa scegliere l'aggiornamento, inserimento ed eliminare le schede](paging-and-sorting-report-data-cs/_static/image5.png)

**Figura 5**: scegliere (nessuno) opzione nell'elenco a discesa scegliere l'aggiornamento, inserimento ed eliminare le schede


Successivamente, consentire s personalizzare i campi di s GridView in modo che vengano visualizzati solo il nomi di prodotti, fornitori, categorie, prezzi e gli Stati non più utilizzati. Inoltre, è possibile apportare qualsiasi formattazione a livello di campo cambia, ad esempio modificando il `HeaderText` proprietà o la formattazione di price come valuta. Dopo tali modifiche, il markup dichiarativo di GridView s dovrebbe essere simile al seguente:


[!code-aspx[Main](paging-and-sorting-report-data-cs/samples/sample2.aspx)]

Figura 6 mostra lo stato di avanzamento fino ad ora, quando viene visualizzato tramite un browser. Si noti che la pagina elenca tutti i prodotti in una schermata che mostra ogni nome di prodotto s, categoria, fornitore, prezzo e lo stato sospeso.


[![Ognuno dei prodotti sono elencati](paging-and-sorting-report-data-cs/_static/image7.png)](paging-and-sorting-report-data-cs/_static/image6.png)

**Figura 6**: ogni prodotto sono elencate ([fare clic per visualizzare l'immagine con dimensioni normali](paging-and-sorting-report-data-cs/_static/image8.png))


## <a name="step-3-adding-paging-support"></a>Passaggio 3: Aggiunta del supporto di Paging

Listato *tutti* dei prodotti in un'unica schermata può causare il sovraccarico di informazioni per l'utente ad analizzare i dati. Per rendere più gestibili i risultati, è possibile suddividere i dati in pagine più piccole di dati e consentire all'utente di scorrere i dati una pagina alla volta. Per eseguire questo è sufficiente selezionare la casella di controllo Attiva Paging nello smart tag s GridView (viene impostata la s GridView [ `AllowPaging` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.allowpaging.aspx) a `true`).


[![Controllo casella di controllo Abilita il Paging per aggiungere il supporto di Paging](paging-and-sorting-report-data-cs/_static/image10.png)](paging-and-sorting-report-data-cs/_static/image9.png)

**Figura 7**: abilitare il Paging la casella di controllo per aggiungere il supporto di Paging ([fare clic per visualizzare l'immagine con dimensioni normali](paging-and-sorting-report-data-cs/_static/image11.png))


Abilitazione di paging limita il numero di record visualizzati per pagina e aggiunge un *interfaccia di paging* a GridView. L'interfaccia di paging predefinito, illustrato nella figura 7, è una serie di numeri di pagina, consentendo all'utente di spostarsi rapidamente da una pagina di dati a un altro. Questa interfaccia di paging dovrebbe essere familiare, come abbiamo ve di rete quando si aggiunge il supporto di paging per i controlli DetailsView e FormView nelle esercitazioni precedenti.

Sia il DetailsView e FormView mostrano solo un singolo record per ogni pagina. Il controllo GridView, tuttavia, consulta relativi [ `PageSize` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.pagesize.aspx) per determinare il numero di record da visualizzare per pagina (impostazione predefinita questa proprietà su un valore pari a 10).

Questa interfaccia di paging s GridView, DetailsView e FormView può essere personalizzata utilizzando le proprietà seguenti:

- `PagerStyle` indica le informazioni sullo stile per l'interfaccia di paging. può specificare impostazioni come `BackColor`, `ForeColor`, `CssClass`, `HorizontalAlign`e così via.
- `PagerSettings` contiene un insieme di proprietà che è possibile personalizzare la funzionalità dell'interfaccia di paging. `PageButtonCount` indica il numero massimo di numeri di pagina numerici visualizzati nell'interfaccia di paging (il valore predefinito è 10); il [ `Mode` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pagersettings.mode.aspx) indica come l'interfaccia di paging viene eseguito e può essere impostata su: 

    - `NextPrevious` Mostra i pulsanti Avanti e indietro, consentendo all'utente di passaggio avanti o indietro una pagina alla volta
    - `NextPreviousFirstLast` Oltre ai pulsanti Avanti e indietro, pulsanti e cognome sono inclusi anche, consentendo all'utente di passare rapidamente a prima o ultima pagina di dati
    - `Numeric` viene illustrata una serie di numeri di pagina, consentendo all'utente di passare immediatamente a una pagina
    - `NumericFirstLast` oltre i numeri di pagina, include i pulsanti e cognome, consentendo all'utente di passare rapidamente a prima o ultima pagina di dati. i pulsanti/cognome vengono visualizzati solo se tutti i numeri di pagina numerico non è sufficiente

Inoltre, il controllo GridView, DetailsView e FormView tutti offrono la `PageIndex` e `PageCount` proprietà, che indicano la pagina corrente che viene visualizzato e il numero totale di pagine di dati, rispettivamente. Il `PageIndex` proprietà è indicizzata a partire da 0, il che significa che quando si visualizzano la prima pagina di dati `PageIndex` sarà uguale a 0. `PageCount`, d'altra parte, inizia il conteggio da 1, vale a dire che `PageIndex` è limitato per i valori compresi tra 0 e `PageCount - 1`.

Let s si consiglia di migliorare l'aspetto predefinito dell'interfaccia di paging s GridView. In particolare, lasciare s che hanno l'interfaccia di paging allineato a destra con uno sfondo grigio chiaro. Anziché impostare queste proprietà direttamente tramite la s GridView `PagerStyle` proprietà, s ti permettono di creare una classe CSS in `Styles.css` denominate `PagerRowStyle` e quindi assegnare il `PagerStyle` s `CssClass` proprietà tramite il nostro tema. Iniziare aprendo `Styles.css` e definizione della classe aggiungendo il codice CSS seguente:


[!code-css[Main](paging-and-sorting-report-data-cs/samples/sample3.css)]

Successivamente, aprire il `GridView.skin` del file nel `DataWebControls` cartella all'interno di `App_Themes` cartella. Come accennato nel *pagine Master e spostamento nel sito* file dell'esercitazione, interfaccia possono essere utilizzati per specificare i valori di proprietà predefiniti per un controllo Web. Pertanto, aumentare le impostazioni esistenti per includere l'impostazione di `PagerStyle` s `CssClass` proprietà `PagerRowStyle`. Inoltre, ti permettono di s configurare l'interfaccia di paging per visualizzare al massimo i pulsanti di cinque pagina numerici usando la `NumericFirstLast` interfaccia di paging.


[!code-aspx[Main](paging-and-sorting-report-data-cs/samples/sample4.aspx)]

## <a name="the-paging-user-experience"></a>L'esperienza utente di Paging

Figura 8 mostra la pagina web quando visitati tramite un browser dopo che è stata selezionata la casella di controllo s Abilita Paging del controllo GridView e il `PagerStyle` e `PagerSettings` configurazioni sono state apportate tramite la `GridView.skin` file. Si noti come solo dieci record vengono visualizzati e l'interfaccia di paging indica che si sta visualizzando la prima pagina di dati.


[![Con Paging abilitata, solo un Subset dei record vengono visualizzati contemporaneamente](paging-and-sorting-report-data-cs/_static/image13.png)](paging-and-sorting-report-data-cs/_static/image12.png)

**Figura 8**: con abilitato il Paging, solo un Subset dei record vengono visualizzati in un momento ([fare clic per visualizzare l'immagine con dimensioni normali](paging-and-sorting-report-data-cs/_static/image14.png))


Quando l'utente fa clic su uno dei numeri di pagina nell'interfaccia di paging, un postback previsioni e la pagina Ricarica con cui pagina s record richiesti. Figura 9 mostra i risultati dopo avere acconsentito esplicitamente per visualizzare la pagina finale dei dati. Si noti che la pagina finale ha solo un record; infatti, sono presenti 81 record in totale, risultante in otto pagine di 10 record per ogni pagina più di una pagina con un unico record.


[![Facendo clic su un numero di pagine determina un Postback e Mostra il Subset appropriato di record](paging-and-sorting-report-data-cs/_static/image16.png)](paging-and-sorting-report-data-cs/_static/image15.png)

**Figura 9**: facendo clic su un numero di pagine determina un Postback e Mostra i Subset dei record appropriati ([fare clic per visualizzare l'immagine con dimensioni normali](paging-and-sorting-report-data-cs/_static/image17.png))


## <a name="paging-s-server-side-workflow"></a>Flusso di lavoro di paging s lato Server

Quando l'utente finale fa clic su un pulsante nell'interfaccia di paging, un postback previsioni e inizia il flusso di lavoro lato server seguente:

1. Il controllo GridView s (o DetailsView o FormView) `PageIndexChanging` viene generato l'evento
2. ObjectDataSource richiede nuovamente *tutte* dei dati dalla classe BLL; s GridView `PageIndex` e `PageSize` i valori delle proprietà vengono utilizzati per determinare quali record restituiti dalla classe BLL devono essere visualizzati in GridView
3. Le s GridView `PageIndexChanged` viene generato l'evento

Nel passaggio 2, ObjectDataSource richiede nuovamente tutti i dati provenienti dall'origine dati. Questo stile di paging è noto come *paging predefinito*, come s il comportamento di paging usata per impostazione predefinita durante l'impostazione di `AllowPaging` proprietà `true`. Con default gestire i controllo Web per dati di paging recupera tutti i record per ogni pagina di dati, anche se solo un subset di record vengono effettivamente eseguito il rendering in HTML che s inviate al browser. Se non viene memorizzato nella cache i dati del database dal livello BLL o ObjectDataSource, il paging predefinito è praticabile per i set di risultati di dimensioni sufficientemente grandi, o per le applicazioni web con molti utenti simultanei.

Nella prossima esercitazione verrà esaminato come implementare *paging personalizzato*. Con paging personalizzato è possibile indicare in modo specifico a ObjectDataSource per recuperare solo lo specifico set di record necessari per la pagina richiesta dei dati. Come è facile immaginare, il paging personalizzato migliora notevolmente l'efficienza del paging dei set di risultati di grandi dimensioni.

> [!NOTE]
> Anche il paging predefinito non è idoneo quando il paging tramite set di risultati sufficientemente elevato o per i siti con molti utenti simultanei, tenere presente che il paging personalizzato richiede altre modifiche e allo scopo di implementare e non è semplice come selezionare una casella di controllo (come valore predefinito è paging). Pertanto, il paging predefinito potrebbe essere la scelta ideale per siti Web con traffico ridotto, piccoli o quando set paging di risultati relativamente piccolo, come s molto più semplice e più veloce da implementare.


Ad esempio, se si sa che mai avremo più di 100 prodotti nel nostro database, il miglioramento delle prestazioni minimo a disposizione il paging personalizzato è probabilmente offset del lavoro richiesto per l'implementazione. Se, tuttavia, un giorno è migliaia o decine di migliaia di prodotti *non* implementare il paging personalizzato sarebbe notevolmente ostacolare la scalabilità dell'applicazione.

## <a name="step-4-customizing-the-paging-experience"></a>Passaggio 4: Personalizzare l'esperienza di Paging

Controlli Web dei dati offrono numerose proprietà che può essere utilizzato per migliorare l'esperienza di spostamento assegnare agli utenti. Il `PageCount` proprietà, ad esempio, indica il numero totale delle pagine, mentre il `PageIndex` proprietà indica la pagina visitata e può essere impostata per spostare rapidamente un utente a una pagina specifica. Per illustrare come usare queste proprietà per migliorare l'esperienza di spostamento assegnare agli utenti, consentire s aggiungere un'etichetta ai controlli Web per la pagina che informa l'utente di indicare la pagina re sta visitando, insieme a un controllo DropDownList che consenta di passare rapidamente a una pagina specificata .

In primo luogo, aggiungere un controllo etichetta Web alla pagina, impostare relativi `ID` proprietà `PagingInformation`e cancellare il `Text` proprietà. Successivamente, creare un gestore eventi per s GridView `DataBound` eventi e aggiungere il codice seguente:


[!code-csharp[Main](paging-and-sorting-report-data-cs/samples/sample5.cs)]

Assegna questo gestore dell'evento il `PagingInformation` etichetta s `Text` proprietà a un messaggio che informa l'utente della pagina che attualmente stanno visitando `Products.PageIndex + 1` quante le pagine totali `Products.PageCount` (viene aggiunto 1 e il `Products.PageIndex` proprietà perché `PageIndex` vengono indicizzati a partire da 0). È possibile scegliere di assegnare questa etichetta s `Text` proprietà nel `DataBound` gestore dell'evento in contrapposizione al `PageIndexChanged` gestore dell'evento perché il `DataBound` evento viene generato ogni volta che i dati sono associati a GridView mentre il `PageIndexChanged` solo gestore di eventi generato quando viene modificato l'indice della pagina. Quando il controllo GridView è inizialmente i dati associati nella prima pagina visita, il `PageIndexChanging` fire t l di eventi (mentre il `DataBound` evento).

Grazie a questa aggiunta, all'utente viene ora visualizzato un messaggio che indica quale pagina che stanno visitando e è il numero di pagine complessivo dei dati non esiste.


[![Il numero di pagina corrente e il numero totale di pagine vengono visualizzati](paging-and-sorting-report-data-cs/_static/image19.png)](paging-and-sorting-report-data-cs/_static/image18.png)

**Figura 10**: vengono visualizzati il numero di pagina corrente e del numero totale di pagine ([fare clic per visualizzare l'immagine con dimensioni normali](paging-and-sorting-report-data-cs/_static/image20.png))


Oltre al controllo etichetta, consentire s anche aggiungere un controllo DropDownList che elenca i numeri di pagina in GridView con la pagina correntemente visualizzata selezionata. L'idea è che l'utente può passare rapidamente dalla pagina corrente a altro semplicemente selezionando il nuovo indice di pagina da DropDownList. Iniziare aggiungendo un controllo DropDownList alla finestra di progettazione, l'impostazione relativa `ID` proprietà `PageList` e selezionando l'opzione Abilita AutoPostBack dal suo smart tag.

Successivamente, tornare al `DataBound` gestore dell'evento e aggiungere il codice seguente:


[!code-csharp[Main](paging-and-sorting-report-data-cs/samples/sample6.cs)]

Questo codice inizia con la cancellazione delle voci del `PageList` DropDownList. Ciò potrebbe sembrare superfluo, poiché una t sarebbe prevede che il numero di pagine da modificare, ma potrebbero utilizzano il sistema simultaneamente, aggiungere o rimuovere i record da altri utenti il `Products` tabella. Questo tipo gli inserimenti o eliminazioni è stato possibile modificare il numero di pagine di dati.

Successivamente, è necessario creare di nuovo i numeri di pagina e avere quello che esegue il mapping a GridView corrente `PageIndex` selezionata per impostazione predefinita. È effettuare questa operazione utilizzando un ciclo da 0 a `PageCount - 1`, aggiungendo un nuovo `ListItem` in ogni iterazione e l'impostazione relativa `Selected` la proprietà su true se l'indice di iterazione corrente è uguale a s GridView `PageIndex` proprietà.

Infine, è necessario creare un gestore eventi per s DropDownList `SelectedIndexChanged` evento, che viene attivato ogni volta che l'utente seleziona un elemento diverso dall'elenco. Per creare questo gestore eventi, è sufficiente fare doppio clic su DropDownList nella finestra di progettazione, quindi aggiungere il codice seguente:


[!code-csharp[Main](paging-and-sorting-report-data-cs/samples/sample7.cs)]

Come illustrato nella figura 11, semplicemente modificando la s GridView `PageIndex` proprietà fa in modo che i dati riassociare a GridView. In s GridView `DataBound` gestore eventi appropriato DropDownList `ListItem` sia selezionata.


[![L'utente viene automaticamente visualizzata la sesta pagina quando selezionano l'elemento dell'elenco a discesa scegliere pagina 6](paging-and-sorting-report-data-cs/_static/image22.png)](paging-and-sorting-report-data-cs/_static/image21.png)

**Figura 11**: l'utente viene automaticamente visualizzata la sesta pagina quando selezionano l'elemento dell'elenco a discesa scegliere pagina 6 ([fare clic per visualizzare l'immagine con dimensioni normali](paging-and-sorting-report-data-cs/_static/image23.png))


## <a name="step-5-adding-bi-directional-sorting-support"></a>Passaggio 5: Aggiunta del supporto di ordinamento bidirezionali

L'aggiunta di supporto bidirezionale è semplice quanto l'aggiunta di supporto di paging è sufficiente selezionare l'opzione Abilita ordinamento dallo smart tag s GridView (che imposta la s GridView [ `AllowSorting` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.allowsorting.aspx) a `true`). Questa modalità rende impossibile ognuna delle intestazioni dei campi s GridView come LinkButton che, quando si fa clic, possono causare un postback e restituire i dati ordinati in base alla colonna selezionata in ordine crescente. Fare di nuovo la stessa intestazione LinkButton Ordina nuovamente i dati in ordine decrescente.

> [!NOTE]
> Se si usa un livello di accesso ai dati personalizzati anziché a un set di dati tipizzato, non si abbia un'opzione Abilita ordinamento nello smart tag s GridView. Solo i GridView associati alle origini dati che in modo nativo supportano l'ordinamento sono questa casella di controllo disponibile. Il set di dati tipizzato fornisce il supporto dell'ordinamento della finestra poiché l'oggetto DataTable di ADO.NET fornisce un `Sort` metodo che, quando richiamata, Ordina gli oggetti DataTable DataRow usando i criteri specificati.


Se DAL non restituisce oggetti che supportano l'ordinamento che sarà necessario configurare ObjectDataSource per passare le informazioni di ordinamento per il livello di logica di Business, che può ordinare i dati o i dati ordinato in modo nativo per DAL. Verrà illustrato come ordinare i dati nella logica di Business e livelli di accesso ai dati in un'esercitazione futura.

L'ordinamento LinkButton vengono visualizzati come collegamenti ipertestuali HTML, i cui colori corrente (blu per un collegamento non visitato e un rosso scuro per un collegamento visitato) in conflitto con il colore di sfondo della riga di intestazione. Let s presenti, invece, tutti i collegamenti di riga di intestazione visualizzati in bianco, indipendentemente dal fatto che essi ve stato visitato o meno. Questa operazione può essere eseguita aggiungendo il codice seguente per il `Styles.css` classe:


[!code-css[Main](paging-and-sorting-report-data-cs/samples/sample8.css)]

Questa sintassi indica di usare testo bianco quando tali collegamenti ipertestuali all'interno di un elemento che utilizza la classe HeaderStyle vengono visualizzati.

Dopo l'aggiunta di CSS, quando si visita la pagina tramite un browser la schermata dovrebbe essere simile alla figura 12. In particolare, nella figura 12 mostra i risultati dopo che è stato fatto clic sul collegamento di intestazione s campo Prezzo.


[![I risultati sono stati ordinati per l'elemento UnitPrice in ordine crescente](paging-and-sorting-report-data-cs/_static/image25.png)](paging-and-sorting-report-data-cs/_static/image24.png)

**Figura 12**: il risultati sono ordinati in base all'elemento UnitPrice in ordine crescente ([fare clic per visualizzare l'immagine con dimensioni normali](paging-and-sorting-report-data-cs/_static/image26.png))


## <a name="examining-the-sorting-workflow"></a>Analisi di flusso di lavoro di ordinamento

GridView tutti i campi i BoundField, CampoCasellaDiControllo, TemplateField e così via hanno un `SortExpression` proprietà che indica l'espressione che deve essere utilizzato per ordinare i dati quando si fa clic sul collegamento di intestazione di ordinamento che il campo s. Include anche un `SortExpression` proprietà. Quando un'intestazione di ordinamento viene fatto clic su LinkButton, GridView assegna tale il campo s `SortExpression` valore relativo `SortExpression` proprietà. Successivamente, i dati vengono recuperati nuovamente da ObjectDataSource e ordinati in base ai dispositivi GridView `SortExpression` proprietà. Nell'elenco seguente illustra nel dettaglio la sequenza di passaggi che accade realmente quando un utente finale consente di ordinare i dati in un controllo GridView:

1. Le s GridView [evento Sorting](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sorting(VS.80).aspx) viene attivato
2. S GridView [ `SortExpression` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sortexpression.aspx) è impostata sul `SortExpression` del campo cui intestazione ordinamento LinkButton selezionato
3. ObjectDataSource nuovamente recupera tutti i dati da BLL e quindi ordina i dati usando i dispositivi di GridView `SortExpression`
4. Le s GridView `PageIndex` proprietà viene reimpostata su 0, vale a dire che l'ordinamento all'utente viene restituito alla prima pagina di dati (presupponendo che è stato implementato il supporto di paging)
5. Le s GridView [ `Sorted` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sorted(VS.80).aspx) viene attivato

Come con il paging predefinito, l'impostazione predefinita l'opzione di ordinamento recupera nuovamente *tutti* dei record da BLL. Quando si usa l'ordinamento senza paging o quando si usa l'ordinamento con non predefinita paging, qui 1!s nessun modo per ovviare a sulle prestazioni (oltre la memorizzazione nella cache i dati del database). Tuttavia, come vedremo in un'esercitazione futura, è possibile ordinare in modo efficiente i dati quando si usa il paging personalizzato.

Quando si associa un ObjectDataSource a GridView mediante l'elenco di riepilogo a discesa nello smart tag s GridView, ogni campo di GridView dispone automaticamente relativi `SortExpression` assegnato al nome del campo dati nella proprietà il `ProductsRow` classe. Ad esempio, il `ProductName` s BoundField `SortExpression` è impostata su `ProductName`, come illustrato nel markup dichiarativo seguenti:


[!code-aspx[Main](paging-and-sorting-report-data-cs/samples/sample9.aspx)]

Un campo può essere configurato in modo che è non ordinabile cancellando s relativo `SortExpression` proprietà (assegnazione a una stringa vuota). Per illustrare questo concetto, si supponga che si desidera consentire ai clienti di ordinare i nostri prodotti in base al prezzo t. Il `UnitPrice` BoundField s `SortExpression` proprietà può essere rimosso dal markup dichiarativo o tramite la finestra di dialogo campi (che è accessibile facendo clic sul collegamento Modifica colonne nello smart tag GridView s).


![I risultati sono stati ordinati per l'elemento UnitPrice in ordine crescente](paging-and-sorting-report-data-cs/_static/image27.png)

**Figura 13**: I risultati sono stati ordinati per l'elemento UnitPrice in ordine crescente


Una volta il `SortExpression` proprietà è stata rimossa la `UnitPrice` BoundField, l'intestazione viene eseguito il rendering come testo anziché come un collegamento, impedendo in tal modo agli utenti di ordinamento dei dati in base al prezzo.


[![Rimuovendo la proprietà SortExpression, gli utenti non sarà più possono ordinare i prodotti in base al prezzo](paging-and-sorting-report-data-cs/_static/image29.png)](paging-and-sorting-report-data-cs/_static/image28.png)

**Figura 14**: rimuovendo la proprietà SortExpression, gli utenti non è più possono ordinare il prezzo per i prodotti ([fare clic per visualizzare l'immagine con dimensioni normali](paging-and-sorting-report-data-cs/_static/image30.png))


## <a name="programmatically-sorting-the-gridview"></a>L'ordinamento a livello di programmazione di GridView

È inoltre possibile ordinare il contenuto di GridView a livello di codice usando la s GridView [ `Sort` metodo](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sort.aspx). Basta passare il `SortExpression` valore per ordinare in base con il [ `SortDirection` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sortdirection.aspx) (`Ascending` o `Descending`), e i dati di s GridView saranno nuovamente ordinati.

Si supponga che il motivo è disattivata l'ordinamento in base il `UnitPrice` era perché siamo rappresentano un problema che i clienti acquisterebbero semplicemente solo i prodotti di prezzo più basso. Tuttavia, si vuole invitali ad acquistare i prodotti più costosi, pertanto è il d, ad esempio essere in grado di ordinare i prodotti in base al prezzo, ma solo dal prezzo più costoso alla meno.

Per eseguire questo aggiungere un controllo pulsante Web alla pagina, impostare relativi `ID` proprietà `SortPriceDescending`e il relativo `Text` proprietà da ordinare in base al prezzo. Successivamente, creare un gestore eventi per il pulsante s `Click` eventi facendo doppio clic sul controllo pulsante nella finestra di progettazione. Aggiungere il codice seguente a questo gestore eventi:


[!code-csharp[Main](paging-and-sorting-report-data-cs/samples/sample10.cs)]

Facendo clic su questo pulsante l'utente torna alla prima pagina con i prodotti ordinati in base al prezzo, dal metodo più costoso meno costoso (vedere Figura 15).


[![Fare clic sul pulsante Ordina i prodotti da del più costoso alla meno](paging-and-sorting-report-data-cs/_static/image32.png)](paging-and-sorting-report-data-cs/_static/image31.png)

**Figura 15**: fare clic sul pulsante Ordina i prodotti da the più dispendiose alla meno ([fare clic per visualizzare l'immagine con dimensioni normali](paging-and-sorting-report-data-cs/_static/image33.png))


## <a name="summary"></a>Riepilogo

In questa esercitazione che è stato illustrato come implementare predefinito funzionalità di ordinamento e paging, entrambi sono stati semplice come selezionare una casella di controllo. Quando un utente consente di ordinare o Sfoglia le pagine di dati, si espande un flusso di lavoro simile:

1. Previsioni di un postback
2. I dati di controllo del codice Web s pre-livello viene generato l'evento (`PageIndexChanging` o `Sorting`)
3. Tutti i dati vengono recuperati nuovamente da ObjectDataSource
4. I dati di controllo del codice Web s di post-livello viene generato l'evento (`PageIndexChanged` o `Sorted`)

Mentre l'implementazione di paging di base e l'ordinamento è un gioco da ragazzi, deve essere esercitato sforzo di utilizzare il paging personalizzato più efficiente o per migliorare ulteriormente l'interfaccia di paging o di ordinamento. Questi argomenti verranno esaminati nelle esercitazioni future.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [avanti](efficiently-paging-through-large-amounts-of-data-cs.md)
