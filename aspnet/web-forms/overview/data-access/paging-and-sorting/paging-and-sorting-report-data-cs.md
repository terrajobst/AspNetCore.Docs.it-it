---
uid: web-forms/overview/data-access/paging-and-sorting/paging-and-sorting-report-data-cs
title: Ordinamento e paging segnalare i dati (c#) | Documenti Microsoft
author: rick-anderson
description: Ordinamento e paging sono due caratteristiche molto comune quando si visualizzano i dati in un'applicazione in linea. In questa esercitazione si verrà esaminato un prima aggiunta di ordinamento e...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2006
ms.topic: article
ms.assetid: 811a6ef2-ec66-4c8e-a089-6f795056e288
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/paging-and-sorting-report-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 06a907f2af0adb2eb8aef5a814c2d767b62db69a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887304"
---
<a name="paging-and-sorting-report-data-c"></a>Il paging e l'ordinamento dei dati di Report (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare App di esempio](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_24_CS.exe) o [Scarica il PDF](paging-and-sorting-report-data-cs/_static/datatutorial24cs1.pdf)

> Ordinamento e paging sono due caratteristiche molto comune quando si visualizzano i dati in un'applicazione in linea. In questa esercitazione si verrà esaminato un primo aggiunta dell'ordinamento e paging per i report, che verrà quindi compilato al momento di esercitazioni in futuro.


## <a name="introduction"></a>Introduzione

Ordinamento e paging sono due caratteristiche molto comune quando si visualizzano i dati in un'applicazione in linea. Ad esempio, quando si cerca documentazione ASP.NET in una libreria online, possono essere presenti centinaia di libri, ma il report che elenca i risultati della ricerca vengono elencati solo dieci corrispondenze per ogni pagina. Inoltre, i risultati possono essere ordinati per titolo, prezzo, conteggio delle pagine, il nome dell'autore e così via. Negli ultimi 23 esercitazioni esaminato come compilare una varietà di report, incluse le interfacce che consentono l'aggiunta, modifica ed eliminazione di dati, mentre è ve non esaminata come ordinare i dati e l'unica esempi di paging è state ve visualizzata con il controllo DetailsView e FormView controlli.

In questa esercitazione vedremo come aggiungere l'ordinamento e paging per i report, che possono essere eseguiti selezionando semplicemente alcune caselle di controllo. Sfortunatamente, questa implementazione semplicistica ha relativi svantaggi che lascia un po' di essere l'interfaccia di ordinamento e le routine di paging non sono progettate per il paging in modo efficiente a set di risultati di grandi dimensioni. Nelle esercitazioni successive verranno illustrato come aggirare le limitazioni di out-of-the-box soluzioni di ordinamento e paging.

## <a name="step-1-adding-the-paging-and-sorting-tutorial-web-pages"></a>Passaggio 1: Aggiungere la funzionalità di Paging e l'ordinamento le pagine Web dell'esercitazione

Prima di iniziare questa esercitazione, lasciare s innanzitutto è opportuno aggiungere le pagine ASP.NET che è necessario per questa esercitazione e i successivi tre. Iniziare creando una nuova cartella nel progetto denominato `PagingAndSorting`. Successivamente, aggiungere le seguenti cinque pagine ASP.NET in questa cartella, con tutti gli elementi configurati per utilizzare la pagina master `Site.master`:

- `Default.aspx`
- `SimplePagingSorting.aspx`
- `EfficientPaging.aspx`
- `SortParameter.aspx`
- `CustomSortingUI.aspx`


![Creare una cartella PagingAndSorting e aggiungere le pagine ASP.NET dell'esercitazione](paging-and-sorting-report-data-cs/_static/image1.png)

**Figura 1**: creare una cartella PagingAndSorting e aggiungere le pagine ASP.NET Tutorial


Aprire quindi il `Default.aspx` pagina e trascinare il `SectionLevelTutorialListing.ascx` controllo utente dal `UserControls` cartella nell'area di progettazione. Questo controllo utente, creata nel [pagine Master e esplorazione del sito](../introduction/master-pages-and-site-navigation-cs.md) esercitazione enumera la mappa del sito e visualizza tali esercitazioni nella sezione corrente in un elenco puntato.


![Aggiungere il controllo utente SectionLevelTutorialListing.ascx Default.aspx](paging-and-sorting-report-data-cs/_static/image2.png)

**Figura 2**: aggiungere il controllo utente SectionLevelTutorialListing.ascx a default. aspx


Per visualizzare l'elenco puntato per lo spostamento e l'ordinamento di esercitazioni che verrà creata, è necessario aggiungerli alla mappa del sito. Aprire il `Web.sitemap` file e quindi aggiungere il markup seguente dopo il markup nodo di mappa del sito di modifica, inserimento ed eliminazione:


[!code-xml[Main](paging-and-sorting-report-data-cs/samples/sample1.xml)]


![Aggiornare la mappa per includere le nuove pagine ASP.NET](paging-and-sorting-report-data-cs/_static/image3.png)

**Figura 3**: aggiornare la mappa per includere le nuove pagine ASP.NET


## <a name="step-2-displaying-product-information-in-a-gridview"></a>Passaggio 2: Visualizzazione di informazioni sui prodotti in un controllo GridView.

Prima di è effettivamente implementare funzionalità di ordinamento e paging, consentire s creare innanzitutto un standard non-srotable, non di paging GridView che elenca le informazioni sul prodotto. Si tratta di un'attività è viene eseguito più volte prima di questa serie di esercitazioni, pertanto questi passaggi è necessario conoscere. Aprire il `SimplePagingSorting.aspx` pagina e trascinare un controllo GridView dalla casella degli strumenti nella finestra di progettazione, l'impostazione relativa `ID` proprietà `Products`. Successivamente, creare un nuovo oggetto ObjectDataSource che utilizza la classe ProductsBLL s `GetProducts()` per restituire tutte le informazioni di prodotto.


![Recuperare informazioni su tutti i prodotti con il metodo GetProducts()](paging-and-sorting-report-data-cs/_static/image4.png)

**Figura 4**: recuperare informazioni su tutti i prodotti con il metodo GetProducts()


Poiché questo report è un report di sola lettura, s non esiste, non è necessario eseguire il mapping di s ObjectDataSource `Insert()`, `Update()`, o `Delete()` metodi corrispondente `ProductsBLL` metodi; pertanto, scegliere (nessuno) dall'elenco a discesa per l'aggiornamento, inserimento, e le schede di eliminazione.


![Scegliere (nessuno) opzione nell'elenco a discesa nell'aggiornamento, inserimento ed eliminare le schede](paging-and-sorting-report-data-cs/_static/image5.png)

**Figura 5**: scegliere (nessuno) opzione nell'elenco a discesa nell'aggiornamento, inserimento ed eliminare le schede


Successivamente, consente s di personalizzare i campi s GridView in modo che vengono visualizzati solo i nomi di prodotti, fornitori, categorie, prezzi e gli Stati non più supportati. Inoltre, è possibile apportare qualsiasi formattazione a livello di campo cambia, ad esempio modificando il `HeaderText` proprietà o la formattazione al prezzo come valuta. Dopo aver apportato queste modifiche, il markup dichiarativo di GridView s dovrebbe essere simile al seguente:


[!code-aspx[Main](paging-and-sorting-report-data-cs/samples/sample2.aspx)]

Figura 6 mostra l'avanzamento finora quando viene visualizzato tramite un browser. Si noti che la pagina vengono elencati tutti i prodotti in una schermata, con ogni nome di prodotto s, categoria, fornitore, prezzo e lo stato sospeso.


[![Ognuno dei prodotti sono elencati](paging-and-sorting-report-data-cs/_static/image7.png)](paging-and-sorting-report-data-cs/_static/image6.png)

**Figura 6**: ogni prodotto elencato ([fare clic per visualizzare l'immagine ingrandita](paging-and-sorting-report-data-cs/_static/image8.png))


## <a name="step-3-adding-paging-support"></a>Passaggio 3: Aggiunta del supporto di Paging

Elenco *tutti* dei prodotti in un'unica schermata può causare il sovraccarico di informazioni per l'utente ad analizzare i dati. Per rendere più gestibili i risultati, è possibile suddividere i dati in pagine di dati di dimensioni ridotte e consentire all'utente di scorrere i dati una pagina alla volta. Per eseguire questo sufficiente selezionare la casella di controllo Abilita Paging dallo smart tag GridView s (imposta s GridView [ `AllowPaging` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.allowpaging.aspx) a `true`).


[![La casella Abilita Paging per aggiungere il supporto di Paging di controllo](paging-and-sorting-report-data-cs/_static/image10.png)](paging-and-sorting-report-data-cs/_static/image9.png)

**Figura 7**: abilitare il Paging la casella di controllo per aggiungere il supporto di Paging ([fare clic per visualizzare l'immagine ingrandita](paging-and-sorting-report-data-cs/_static/image11.png))


Abilitazione di paging limita il numero di record visualizzati per pagina e aggiunge un *interfaccia di paging* a GridView. L'interfaccia di paging predefinito, illustrato nella figura 7, è una serie di numeri di pagina, consentendo all'utente di spostarsi rapidamente da una pagina di dati a un altro. Questa interfaccia di paging dovrebbe essere familiare, qualora vi viene visualizzato quando si aggiunge il supporto di paging per i controlli DetailsView e FormView nelle esercitazioni precedenti.

Controlli di DetailsView sia FormView mostrano solo un singolo record per ogni pagina. GridView, tuttavia, consulta la [ `PageSize` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.pagesize.aspx) per determinare il numero di record in modo da visualizzare per pagina (impostazione predefinita questa proprietà su un valore pari a 10).

Questa interfaccia di paging GridView, DetailsView e FormView s può essere personalizzata tramite le proprietà seguenti:

- `PagerStyle` indica le informazioni sullo stile per l'interfaccia di paging. possono specificare le impostazioni `BackColor`, `ForeColor`, `CssClass`, `HorizontalAlign`e così via.
- `PagerSettings` contiene un insieme di proprietà che è possibile personalizzare la funzionalità dell'interfaccia di paging. `PageButtonCount` indica il numero massimo di numeri di pagina numerici visualizzati nell'interfaccia di paging (il valore predefinito è 10); il [ `Mode` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pagersettings.mode.aspx) indica come l'interfaccia di paging opera e può essere impostato su: 

    - `NextPrevious` Mostra pulsanti Avanti e indietro, consentendo all'utente di passo avanti o indietro una pagina alla volta
    - `NextPreviousFirstLast` Oltre ai pulsanti Avanti e indietro, primo e ultimo pulsanti sono inclusi anche, consentendo all'utente di spostarsi rapidamente alla prima o nell'ultima pagina di dati
    - `Numeric` Mostra una serie di numeri di pagina, consentendo all'utente di passare immediatamente a una pagina
    - `NumericFirstLast` Oltre ai numeri di pagina, include i pulsanti First e Last, consentendo all'utente di passare rapidamente a prima o ultima pagina di dati. i pulsanti First o Last vengono visualizzati solo se tutti i numeri di pagina numerici non è sufficiente

Inoltre, il GridView, DetailsView e FormView offrono il `PageIndex` e `PageCount` proprietà, che indicano la pagina corrente visualizzato e il numero totale di pagine di dati, rispettivamente. Il `PageIndex` proprietà è indicizzata a partire da 0, il che significa che quando si visualizzano la prima pagina di dati `PageIndex` sarà uguale a 0. `PageCount`, invece, Avvia conteggio da 1, il che significa che `PageIndex` è limitato per i valori compresi tra 0 e `PageCount - 1`.

Consente di dedicare alcuni minuti per migliorare l'aspetto predefinito dell'interfaccia di paging di GridView s s. In particolare, consentono s che nell'interfaccia di paging allineati a destra con uno sfondo grigio chiaro. Anziché l'impostazione di queste proprietà direttamente tramite s GridView `PagerStyle` proprietà s consentono di creare una classe CSS in `Styles.css` denominato `PagerRowStyle` e quindi assegnare il `PagerStyle` s `CssClass` proprietà tramite il tema. Avvia aprendo `Styles.css` e definizione di classe aggiungere il codice CSS seguente:


[!code-css[Main](paging-and-sorting-report-data-cs/samples/sample3.css)]

Aprire quindi il `GridView.skin` file nel `DataWebControls` cartella all'interno di `App_Themes` cartella. Come accennato nel *pagine Master e esplorazione del sito* file dell'esercitazione, interfaccia possono essere utilizzati per specificare i valori di proprietà predefiniti per un controllo Web. Pertanto, aumentare le impostazioni esistenti per includere l'impostazione di `PagerStyle` s `CssClass` proprietà `PagerRowStyle`. Inoltre, s consentono di configurare l'interfaccia di paging da visualizzare al massimo i pulsanti pagina numerici cinque utilizzando il `NumericFirstLast` interfaccia di paging.


[!code-aspx[Main](paging-and-sorting-report-data-cs/samples/sample4.aspx)]

## <a name="the-paging-user-experience"></a>L'esperienza utente di Paging

Figura 8 è illustrata la pagina web quando visitato tramite un browser dopo la casella di controllo s Abilita Paging del controllo GridView. è stato archiviato e `PagerStyle` e `PagerSettings` configurazioni sono state apportate tramite la `GridView.skin` file. Si noti come solo dieci record vengono visualizzati e l'interfaccia di paging indica che si sta visualizzando la prima pagina di dati.


[![Con Paging abilitata, vengono visualizzati solo un Subset dei record alla volta](paging-and-sorting-report-data-cs/_static/image13.png)](paging-and-sorting-report-data-cs/_static/image12.png)

**Figura 8**: con Paging Enabled, solo un Subset dei record vengono visualizzati in un momento ([fare clic per visualizzare l'immagine ingrandita](paging-and-sorting-report-data-cs/_static/image14.png))


Quando l'utente fa clic su uno dei numeri di pagina nell'interfaccia di paging, viene utilizzata un postback e la pagina Ricarica che mostra che la pagina s record richiesti. Figura 9 mostra i risultati dopo avere acconsentito per visualizzare la pagina finale di dati. Si noti che la pagina finale ha solo un record. infatti, sono presenti 81 record in totale, risultante in otto pagine di 10 record per ogni pagina più di una pagina con un unico record.


[![Facendo clic su un numero di pagine provoca un Postback e viene illustrato il Subset appropriato di record](paging-and-sorting-report-data-cs/_static/image16.png)](paging-and-sorting-report-data-cs/_static/image15.png)

**Figura 9**: facendo clic su un numero di pagina provoca un Postback e Mostra i Subset di record ([fare clic per visualizzare l'immagine ingrandita](paging-and-sorting-report-data-cs/_static/image17.png))


## <a name="paging-s-server-side-workflow"></a>Flusso di lavoro di paging s sul lato Server

Quando l'utente finale fa clic su un pulsante nell'interfaccia di paging, viene utilizzata un postback e inizia il flusso di lavoro sul lato server seguente:

1. Il controllo GridView. s (o DetailsView o FormView) `PageIndexChanging` viene generato l'evento
2. ObjectDataSource nuovamente richieste *tutti* dei dati da BLL; s GridView `PageIndex` e `PageSize` i valori delle proprietà vengono utilizzati per determinare quali record restituiti da BLL devono essere visualizzati in GridView
3. S GridView `PageIndexChanged` viene generato l'evento

Nel passaggio 2, ObjectDataSource richiede nuovamente tutti i dati dall'origine dati. Questo stile di paging è considerato come *paging predefinito*, come il comportamento di paging usato per impostazione predefinita quando l'impostazione di `AllowPaging` proprietà `true`. Con l'impostazione predefinita il paging dei dati di controllo Web gestire recupera tutti i record per ogni pagina di dati, anche se solo un subset di record vengono effettivamente eseguito il rendering in HTML che s inviata al browser. A meno che non viene memorizzato nella cache di dati del database da ObjectDataSource o BLL, paging predefinito risulta inaccettabile per set di risultati sufficientemente grande o per le applicazioni web con molti utenti simultanei.

Nella prossima esercitazione verrà esaminato come implementare *il paging personalizzato*. Con il paging personalizzato è possibile indicare in modo specifico ObjectDataSource per recuperare solo lo specifico set di record necessari per la pagina richiesta dei dati. Come è facile immaginare, il paging personalizzato migliora l'efficienza di paging di set di risultati di grandi dimensioni.

> [!NOTE]
> Paging predefinito non è idoneo quando il paging di set di risultati sufficientemente grande o per i siti con molti utenti simultanei, tenere presente che il paging personalizzato richiede ulteriori modifiche e impegno per implementare e non è semplice come una casella di controllo (come valore predefinito è paging). Pertanto, paging predefinito potrebbe essere la scelta ideale per siti Web di piccole dimensioni, il traffico di basso o quando imposta paging risultati di dimensioni relativamente ridotte, come s molto più semplice e rapido implementare.


Ad esempio, se si sa che non sono più di 100 prodotti nel database, il miglioramento delle prestazioni minimo per il paging personalizzato è probabilmente offset del lavoro richiesto per l'implementazione. Se, tuttavia, un giorno è migliaia o decine di migliaia di prodotti, *non* implementare il paging personalizzato sarebbe notevolmente ostacolare la scalabilità dell'applicazione.

## <a name="step-4-customizing-the-paging-experience"></a>Passaggio 4: Personalizzare l'esperienza di Paging

I controlli Web di dati specificare un numero di proprietà che può essere utilizzato per migliorare l'esperienza di paging utente s. Il `PageCount` proprietà, ad esempio, indica il numero totale delle pagine, mentre il `PageIndex` proprietà indica la pagina visitata e può essere impostata per spostare rapidamente un utente a una pagina specifica. Per illustrare come utilizzare queste proprietà per migliorare l'esperienza dell'utente s paging, consente di aggiungere un'etichetta s Web controllo alla pagina che informa l'utente di indicare la pagina re attualmente visita, insieme a un controllo DropDownList che consente di passare rapidamente a una pagina specificata .

In primo luogo, aggiungere un controllo etichetta Web alla pagina, impostare il relativo `ID` proprietà `PagingInformation`e cancellare la `Text` proprietà. Successivamente, creare un gestore eventi per s GridView `DataBound` eventi e aggiungere il codice seguente:


[!code-csharp[Main](paging-and-sorting-report-data-cs/samples/sample5.cs)]

Questo gestore eventi viene assegnato il `PagingInformation` etichetta s `Text` proprietà a un messaggio che informa l'utente della pagina che attualmente il `Products.PageIndex + 1` fuori il numero totale di pagine `Products.PageCount` (aggiungere da 1 al `Products.PageIndex` proprietà perché `PageIndex` viene indicizzato a partire da 0). Si è scelto di assegnare questa etichetta s `Text` proprietà nel `DataBound` gestore anziché il `PageIndexChanged` gestore dell'evento perché il `DataBound` evento viene generato ogni volta che i dati sono associati a GridView, mentre il `PageIndexChanged` solo gestore di evento generato quando viene modificato l'indice della pagina. Visitare quando GridView è inizialmente i dati associati nella prima pagina, il `PageIndexChanging` incendio di eventi t (mentre il `DataBound` evento).

Con l'aggiunta, l'utente viene visualizzato un messaggio che indica quale pagina visitato e sono il numero totale di pagine di dati non esiste.


[![Vengono visualizzati il numero di pagina corrente e il numero totale di pagine](paging-and-sorting-report-data-cs/_static/image19.png)](paging-and-sorting-report-data-cs/_static/image18.png)

**Figura 10**: vengono visualizzati il numero di pagina corrente e del numero totale di pagine ([fare clic per visualizzare l'immagine ingrandita](paging-and-sorting-report-data-cs/_static/image20.png))


Oltre al controllo etichetta, consentire s anche aggiungere un controllo DropDownList in cui sono elencati i numeri di pagina in GridView con la pagina visualizzata selezionata. L'idea è che l'utente può passare rapidamente dalla pagina corrente a altro semplicemente selezionando il nuovo indice di pagina da DropDownList. Per iniziare, aggiungere un controllo DropDownList nella finestra di progettazione, l'impostazione relativa `ID` proprietà `PageList` e controllando l'opzione di attivare un postback automatico dal suo smart tag.

Successivamente, ripristinare il `DataBound` gestore dell'evento e aggiungere il codice seguente:


[!code-csharp[Main](paging-and-sorting-report-data-cs/samples/sample6.cs)]

Questo codice inizia cancellando gli elementi di `PageList` DropDownList. Ciò potrebbe sembrare superflua, poiché una t sarebbe prevede che il numero di pagine per modificare, ma è possibile utilizzano simultaneamente il sistema, aggiunta o la rimozione dei record da altri utenti di `Products` tabella. Gli inserimenti o eliminazioni di questo tipo è possibile modificare il numero di pagine di dati.

Successivamente, è necessario creare di nuovo i numeri di pagina e uno che esegue il mapping a GridView corrente `PageIndex` selezionata per impostazione predefinita. Ottenere questo risultato con un ciclo da 0 a `PageCount - 1`, aggiunta di un nuovo `ListItem` in ogni iterazione e l'impostazione relativa `Selected` la proprietà su true se l'indice di iterazione corrente è uguale a s GridView `PageIndex` proprietà.

Infine, è necessario creare un gestore eventi per i dispositivi DropDownList `SelectedIndexChanged` evento, che viene generato ogni volta che l'utente seleziona un elemento diverso dall'elenco. Per creare questo gestore eventi, è sufficiente fare doppio clic su DropDownList nella finestra di progettazione, quindi aggiungere il codice seguente:


[!code-csharp[Main](paging-and-sorting-report-data-cs/samples/sample7.cs)]

Come mostrato nella figura 11, semplicemente modificando il s GridView `PageIndex` fa sì che i dati torni a GridView. In GridView s `DataBound` gestore eventi appropriato DropDownList `ListItem` è selezionata.


[![L'utente viene automaticamente visualizzata la sesta pagina quando selezionando l'elemento di elenco elenco a discesa di pagina 6](paging-and-sorting-report-data-cs/_static/image22.png)](paging-and-sorting-report-data-cs/_static/image21.png)

**Figura 11**: l'utente viene automaticamente visualizzata la sesta pagina quando selezionando l'elemento di elenco elenco a discesa di pagina 6 ([fare clic per visualizzare l'immagine ingrandita](paging-and-sorting-report-data-cs/_static/image23.png))


## <a name="step-5-adding-bi-directional-sorting-support"></a>Passaggio 5: Aggiunta del supporto di ordinamento bidirezionale

Aggiungendo il supporto dell'ordinamento bidirezionale è semplice come l'aggiunta del supporto di paging è sufficiente selezionare l'opzione Abilita ordinamento dallo smart tag GridView s (che imposta il s GridView [ `AllowSorting` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.allowsorting.aspx) a `true`). Si esegue il rendering ognuna delle intestazioni dei campi s GridView come LinkButton che, quando si fa clic, provocare un postback e restituire i dati ordinati in base alla colonna selezionata in ordine crescente. Fare di nuovo la stessa intestazione LinkButton nuovamente Ordina i dati in ordine decrescente.

> [!NOTE]
> Se si utilizza un livello di accesso ai dati personalizzati anziché un DataSet tipizzato, non è possibile un'opzione Abilita ordinamento nello smart tag GridView s. Solo i GridView associati a origini dati che supportano in modo nativo l'ordinamento sono questa casella di controllo disponibile. Il DataSet tipizzato fornisce il supporto dell'ordinamento della casella poiché l'oggetto DataTable di ADO.NET fornisce un `Sort` metodo che, quando richiamata, Ordina gli oggetti DataTable DataRow tramite i criteri specificati.


Se DAL non restituisce gli oggetti in modo nativo supportano l'ordinamento che è necessario configurare ObjectDataSource per passare informazioni sull'ordinamento a livello di logica di Business, che è possibile ordinare i dati o i dati ordinato con DAL. Verrà illustrato come ordinare i dati nella logica di Business e i livelli di accesso ai dati in un'esercitazione future.

L'ordinamento LinkButton vengono visualizzati come collegamenti ipertestuali HTML, i cui colori corrente (blu per un collegamento non visitato e un rosso scuro per un collegamento visitato) conflitti con il colore di sfondo della riga di intestazione. Infatti consentono s dispongono di tutti i collegamenti di riga di intestazione visualizzati in bianco, indipendentemente dal fatto che essi ve stato visitato o non. Questa operazione può essere eseguita aggiungendo il comando seguente per la `Styles.css` classe:


[!code-css[Main](paging-and-sorting-report-data-cs/samples/sample8.css)]

Questa sintassi indica l'utilizzo di testo bianco quando vengono visualizzati i collegamenti ipertestuali all'interno di un elemento che utilizza la classe HeaderStyle.

Dopo l'aggiunta di CSS, visitando la pagina tramite un browser la schermata dovrebbe essere simile alla figura 12. In particolare, nella figura 12 illustra i risultati dopo il collegamento di intestazione s campo Prezzo è stato fatto clic.


[![I risultati sono stati ordinati per il prezzo unitario in ordine crescente](paging-and-sorting-report-data-cs/_static/image25.png)](paging-and-sorting-report-data-cs/_static/image24.png)

**Figura 12**: il risultati sono ordinati in base a UnitPrice in ordine crescente ([fare clic per visualizzare l'immagine ingrandita](paging-and-sorting-report-data-cs/_static/image26.png))


## <a name="examining-the-sorting-workflow"></a>Esaminare il flusso di lavoro di ordinamento

GridView tutti i campi BoundField CheckBoxField, TemplateField e così via sono un `SortExpression` proprietà che indica l'espressione che deve essere utilizzato per ordinare i dati quando si fa clic s tale campo collegamento di intestazione di ordinamento. Dispone anche di GridView un `SortExpression` proprietà. Quando un'intestazione di ordinamento viene fatto clic su LinkButton GridView assegna che il campo s `SortExpression` valore relativo `SortExpression` proprietà. Successivamente, i dati vengono recuperati nuovamente da ObjectDataSource e ordinati in base ai dispositivi GridView `SortExpression` proprietà. Nell'elenco seguente illustra nel dettaglio la sequenza di passaggi che risulti quando un utente finale ordina i dati in un controllo GridView:

1. S GridView [evento Sorting](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sorting(VS.80).aspx) generato
2. S GridView [ `SortExpression` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sortexpression.aspx) è impostato sul `SortExpression` del campo la cui intestazione ordinamento LinkButton selezionato
3. ObjectDataSource nuovamente recupera tutti i dati da BLL, quindi ordina i dati utilizzando gli oggetti di GridView `SortExpression`
4. S GridView `PageIndex` proprietà viene reimpostata su 0, il che significa che durante l'ordinamento dell'utente viene restituito alla prima pagina di dati (presupponendo che sia stato implementato il supporto del paging)
5. S GridView [ `Sorted` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sorted(VS.80).aspx) generato

Come con paging predefinito, l'impostazione predefinita l'opzione di ordinamento recupera nuovamente *tutti* dei record di BLL. Quando si utilizza l'ordinamento senza paging o quando si utilizza l'ordinamento con non predefinito di paging, s esiste alcun modo per ovviare a questo oggetto prestazioni (se non si la memorizzazione nella cache i dati del database). Tuttavia, come vedremo in un'esercitazione in futura, è possibile ordinare in modo efficiente i dati quando si usa il paging personalizzato s.

Quando si associa un ObjectDataSource a GridView tramite l'elenco a discesa nello smart tag GridView s, ogni campo di GridView dispone automaticamente relativo `SortExpression` assegnato al nome del campo dati nella proprietà di `ProductsRow` classe. Ad esempio, il `ProductName` BoundField s `SortExpression` è impostato su `ProductName`, come illustrato nel seguente codice dichiarativo:


[!code-aspx[Main](paging-and-sorting-report-data-cs/samples/sample9.aspx)]

Un campo può essere configurato in modo che è non ordinabile cancellando s relativo `SortExpression` proprietà (l'assegnazione a una stringa vuota). Per illustrare questo concetto, si supponga che si desidera consentire ai clienti di ordinare prodotti in base al prezzo non. Il `UnitPrice` BoundField s `SortExpression` proprietà può essere rimossi dal markup dichiarativo o tramite la finestra di dialogo campi (che è accessibile facendo clic sul collegamento Modifica colonne nello smart tag GridView s).


![I risultati sono stati ordinati per il prezzo unitario in ordine crescente](paging-and-sorting-report-data-cs/_static/image27.png)

**Figura 13**: I risultati sono stati ordinati da UnitPrice in ordine crescente


Una volta il `SortExpression` proprietà è stata rimossa per il `UnitPrice` BoundField, l'intestazione viene eseguito il rendering come testo anziché come un collegamento, impedendo in tal modo agli utenti di ordinamento dei dati in base al prezzo.


[![Quando si rimuove la proprietà SortExpression, gli utenti non è più possono ordinare i prodotti in base al prezzo](paging-and-sorting-report-data-cs/_static/image29.png)](paging-and-sorting-report-data-cs/_static/image28.png)

**Figura 14**: quando si rimuove la proprietà SortExpression, gli utenti non possono ordinare non è più prodotti dal prezzo ([fare clic per visualizzare l'immagine ingrandita](paging-and-sorting-report-data-cs/_static/image30.png))


## <a name="programmatically-sorting-the-gridview"></a>Ordinamento a livello di codice il controllo GridView.

È inoltre possibile ordinare il contenuto del controllo GridView. a livello di codice utilizzando il controllo GridView. [ `Sort` metodo](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sort.aspx). Passare il `SortExpression` valore di ordinamento con il [ `SortDirection` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sortdirection.aspx) (`Ascending` o `Descending`), e i dati di GridView s sarà nuovamente ordinati.

Si supponga che il motivo è disattivata l'ordinamento in base il `UnitPrice` perché siamo teme che i clienti verrebbero semplicemente acquistare solo i prodotti di prezzo più basso. Tuttavia, si vuole incoraggiarli per acquistare i prodotti più costosi, in modo d vorremmo possano essere in grado di ordinare i prodotti in base al prezzo, ma solo dal prezzo più elevato per il minor.

Per eseguire questo aggiungere un controllo pulsante Web alla pagina, impostare il relativo `ID` proprietà `SortPriceDescending`e il relativo `Text` proprietà per ordinare in base al prezzo. Successivamente, creare un gestore eventi per il pulsante s `Click` evento facendo doppio clic sul pulsante nella finestra di progettazione. A questo gestore eventi, aggiungere il codice seguente:


[!code-csharp[Main](paging-and-sorting-report-data-cs/samples/sample10.cs)]

Fare clic su questo pulsante restituisce l'utente alla prima pagina con i prodotti ordinati in base al prezzo, dalla più costosa meno costosa (vedere Figura 15).


[![Fare clic sul pulsante Ordina i prodotti da più costosi per il minor](paging-and-sorting-report-data-cs/_static/image32.png)](paging-and-sorting-report-data-cs/_static/image31.png)

**Figura 15**: fare clic sul pulsante Ordina i prodotti dalla più costosa per il minor ([fare clic per visualizzare l'immagine ingrandita](paging-and-sorting-report-data-cs/_static/image33.png))


## <a name="summary"></a>Riepilogo

In questa esercitazione che è stato illustrato come implementare l'impostazione predefinita le funzionalità di ordinamento e paging, entrambi i quali sono stati semplice come una casella di controllo. Quando un utente Ordina o pagine di dati, si espande un flusso di lavoro simile:

1. Viene utilizzata un postback
2. I dati di controllo Web s pre-livello viene generato l'evento (`PageIndexChanging` o `Sorting`)
3. Tutti i dati vengono recuperati nuovamente da ObjectDataSource
4. I dati di controllo Web s post-livello evento genera (`PageIndexChanged` o `Sorted`)

Durante l'implementazione di paging di base e l'ordinamento è molto semplice, più complessa deve esercitato utilizzare il paging personalizzato più efficiente o per migliorare ulteriormente l'interfaccia di paging o di ordinamento. Questi argomenti verranno esaminati nelle esercitazioni future.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [avanti](efficiently-paging-through-large-amounts-of-data-cs.md)
