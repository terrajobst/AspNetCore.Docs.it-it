---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-cs
title: Visualizzazione di dati con i controlli DataList e Repeater (c#) | Microsoft Docs
author: rick-anderson
description: Nelle esercitazioni precedenti è stato utilizzato il controllo GridView per visualizzare i dati. A partire da questa esercitazione, si esamina la creazione di modelli di report comuni con...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: 0591cacc-b34b-4cf6-885e-2c9953bb0946
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: ca585c51ae12be2395897044f6d0fd1857d3e357
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382702"
---
<a name="displaying-data-with-the-datalist-and-repeater-controls-c"></a>Visualizzazione di dati con i controlli DataList e Repeater (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'App di esempio](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_29_CS.exe) o [Scarica il PDF](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/datatutorial29cs1.pdf)

> Nelle esercitazioni precedenti è stato utilizzato il controllo GridView per visualizzare i dati. A partire da questa esercitazione, si esamina la creazione di modelli di report comuni con i controlli DataList e Repeater, partire dalle nozioni fondamentali di visualizzazione dei dati con questi controlli.


## <a name="introduction"></a>Introduzione

In tutti gli esempi in tutto il passato 28 esercitazioni, in caso di necessità visualizzare più record da un'origine dati è attivate per il controllo GridView. Il controllo GridView viene eseguito il rendering di una riga per ogni record dell'origine dati, visualizzazione dei campi dati il record s nelle colonne. Mentre il controllo GridView rende un gioco da ragazzi display, paging, ordinamento, edit e delete data, l'aspetto del controllo è un po' squadrato. Inoltre, il markup responsabile per la struttura di s GridView è fissa, include un elemento HTML `<table>` con una riga della tabella (`<tr>`) per ogni record e una cella della tabella (`<td>`) per ogni campo.

Per offrire un livello maggiore di personalizzazione nell'aspetto e il markup sottoposto a rendering quando si visualizzano più record, ASP.NET 2.0 offre i controlli DataList e Repeater (che erano anche disponibile in versione di ASP.NET 1.x). I controlli DataList e Repeater eseguire il rendering di contenuto usando i modelli anziché BoundField CheckBoxFields, ButtonFields e così via. Come per GridView, DataList esegue il rendering come HTML `<table>`, ma consente i dati di più record di origine da visualizzare per ogni riga della tabella. Il controllo Repeater, d'altra parte, esegue il rendering di alcun markup aggiuntive rispetto a quella in modo esplicito specificare e sono un candidato ideale quando è necessario un controllo preciso il markup generato.

Tramite le esercitazioni successivamente decina, esamineremo la creazione di modelli di report comuni con i controlli DataList e Repeater, partire dalle nozioni fondamentali di visualizzazione dei dati con questi modelli di controlli. Si vedrà come formattare questi controlli, come modificare il layout dei record di origine dati in DataList, scenari comuni di dettagli/master, modi per modificare ed eliminare dati, come spostarsi tra i record e così via.

## <a name="step-1-adding-the-datalist-and-repeater-tutorial-web-pages"></a>Passaggio 1: Aggiunta di DataList e Repeater esercitazione Web Pages

Prima di iniziare questa esercitazione, lasciare s prima di tutto si consiglia di aggiungere le pagine ASP.NET, che sarà necessario per questa esercitazione e le esercitazioni di alcuni successive trattano di visualizzazione dei dati con DataList e Repeater. Iniziare creando una nuova cartella nel progetto denominato `DataListRepeaterBasics`. Successivamente, aggiungere le seguenti cinque pagine ASP.NET per questa cartella, con tutti gli elementi configurati per utilizzare la pagina master `Site.master`:

- `Default.aspx`
- `Basics.aspx`
- `Formatting.aspx`
- `RepeatColumnAndDirection.aspx`
- `NestedControls.aspx`


![Creare una cartella DataListRepeaterBasics e aggiungere le pagine ASP.NET dell'esercitazione](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image1.png)

**Figura 1**: creare un `DataListRepeaterBasics` cartella e aggiungere le pagine ASP.NET dell'esercitazione


Aprire il `Default.aspx` pagina e trascinare il `SectionLevelTutorialListing.ascx` controllo utente dal `UserControls` cartella nell'area di progettazione. Questo controllo utente, che è stato creato nel [pagine Master e spostamento nel sito](../introduction/master-pages-and-site-navigation-cs.md) esercitazione enumera la mappa del sito e consente di visualizzare le esercitazioni nella sezione corrente in un elenco puntato.


[![Aggiungere il controllo utente sectionleveltutoriallisting. ascx a default. aspx](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image3.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image2.png)

**Figura 2**: aggiungere i `SectionLevelTutorialListing.ascx` controllo utente da `Default.aspx` ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image4.png))


Per visualizzare l'elenco puntato le esercitazioni di DataList e Repeater che verrà creato, è necessario aggiungerli alla mappa del sito. Aprire il `Web.sitemap` file e aggiungere il markup seguente dopo che il markup di nodo della mappa del sito all'aggiunta di pulsanti personalizzati:


[!code-xml[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample1.xml)]


![Aggiornare la mappa del sito in modo da includere le nuove pagine ASP.NET](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image5.png)

**Figura 3**: aggiornare la mappa del sito in modo da includere le nuove pagine ASP.NET


## <a name="step-2-displaying-product-information-with-the-datalist"></a>Passaggio 2: Visualizzare le informazioni di prodotto con DataList

Analogamente a FormView, il controllo DataList s output sottoposto a rendering dipende dalla fase modelli anziché BoundField CheckBoxFields e così via. A differenza di FormView, DataList è progettato per visualizzare un set di record invece di uno solitari. Let s iniziare questa esercitazione con uno sguardo alle informazioni sul prodotto di associazione a un controllo DataList. Iniziare aprendo il `Basics.aspx` nella pagina di `DataListRepeaterBasics` cartella. Successivamente, trascinare un controllo DataList dalla casella degli strumenti nella finestra di progettazione. Come illustrato nella figura 4, prima di specificare i modelli di DataList s, la finestra di progettazione vengono visualizzati come un riquadro grigio.


[![Trascinare il controllo DataList dalla casella degli strumenti nella finestra di progettazione](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image7.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image6.png)

**Figura 4**: trascinare il DataList da di casella degli strumenti in the Designer ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image8.png))


Da s DataList degli smart tag, aggiungere un nuovo oggetto ObjectDataSource e configurarlo in modo da usare la `ProductsBLL` classe s `GetProducts` (metodo). Poiché nuovamente la creazione di un controllo DataList di sola lettura in questa esercitazione impostiamo l'elenco a discesa su (nessuno) in Creazione guidata s inserimento, aggiornamento ed eliminazione di schede.


[![Scegliere di creare un nuovo oggetto ObjectDataSource](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image10.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image9.png)

**Figura 5**: consenso esplicito per creare un nuovo oggetto ObjectDataSource ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image11.png))


[![Configurare ObjectDataSource per usare la classe ProductsBLL](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image13.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image12.png)

**Figura 6**: configurare ObjectDataSource per usare il `ProductsBLL` classe ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image14.png))


[![Recuperare informazioni su tutti i prodotti usando il metodo GetProducts](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image16.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image15.png)

**Figura 7**: recuperare informazioni su tutti di prodotti utilizzando il `GetProducts` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image17.png))


Dopo la configurazione di ObjectDataSource e associarlo a DataList tramite relativo smart tag, Visual Studio creerà automaticamente un `ItemTemplate` in DataList che visualizza il nome e il valore di ogni campo di dati restituito dall'origine dati (vedere la markup riportato di seguito). Questa impostazione predefinita `ItemTemplate` aspetto è identico a quello dei modelli creati automaticamente quando si associa un'origine dati a FormView tramite la finestra di progettazione.


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample2.aspx)]

> [!NOTE]
> È importante ricordare che quando si associa un'origine dati a un controllo FormView tramite lo smart tag s di FormView, Visual Studio ha creato un' `ItemTemplate`, `InsertItemTemplate`, e `EditItemTemplate`. Con il DataList, tuttavia, solo un `ItemTemplate` viene creato. Questo avviene perché DataList non ha le stesse incorporate modifica e inserimento supporto offerto da FormView. DataList contengono eventi correlati a edit e delete e modifica e l'eliminazione di supporto non possono essere aggiunti con un po' di codice, ma non vi s alcun supporto out-of-the-box semplice come con il controllo FormView. Si vedrà come includere modifica ed eliminazione di supporto con il DataList in un'esercitazione futura.


Let s si consiglia di migliorare l'aspetto di questo modello. Anziché visualizzare tutti i campi di dati, consentire s consente di visualizzare solo la s nome prodotto, fornitore, categoria, quantità per l'unità e il prezzo unitario. Inoltre, s ti permettono di visualizzare il nome in un' `<h4>` intestazione e disporre i campi rimanenti tramite un `<table>` sotto l'intestazione.

Per apportare queste modifiche è possibile utilizzare la funzionalità nella finestra di progettazione da DataList s smart tag fare clic sul collegamento di modifica modelli oppure è possibile modificare il modello manualmente tramite la sintassi dichiarativa s della pagina di modifica modelli. Se si usa l'opzione di modifica modelli nella finestra di progettazione, il markup seguente potrebbe non corrispondere esattamente al markup risulta, ma quando viene visualizzato tramite un browser dovrebbe essere molto simile allo screenshot illustrato nella figura 8.


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample3.aspx)]

> [!NOTE]
> L'esempio precedente Usa Web Label controlli la cui `Text` proprietà viene assegnato il valore della sintassi di associazione dati. In alternativa, è stato possibile sono omessi le etichette completamente, digitando semplicemente la sintassi di associazione dati. Vale a dire, anziché usare `<asp:Label ID="CategoryNameLabel" runat="server" Text='<%# Eval("CategoryName") %>' />` si sarebbe potuto invece usare la sintassi dichiarativa `<%# Eval("CategoryName") %>`.


Lasciando nei controlli Web di etichetta, tuttavia, offrono due vantaggi. In primo luogo, fornisce un mezzo più semplice per la formattazione dei dati in base ai dati, come vedremo nella prossima esercitazione. In secondo luogo, l'opzione di modifica modelli nella sintassi della finestra di progettazione t visualizzazione associazione dati dichiarativa che viene visualizzata di fuori di un controllo Web. Al contrario, l'interfaccia di modifica modelli è progettato per facilitare l'uso con markup statici e Web controlli e si presuppone che qualsiasi associazione dati avverrà tramite la finestra di dialogo Modifica DataBindings, accessibile dal Web controlli smart tag.

Pertanto, quando si lavora con DataList, che offre la possibilità di modificare i modelli tramite la finestra di progettazione, è preferibile usare i controlli Web di etichetta in modo che il contenuto è accessibile tramite l'interfaccia di modifica modelli. Come vedremo tra breve, il controllo Repeater richiede che il contenuto del modello s essere modificato dalla vista origine. Di conseguenza, durante la creazione di modelli di s Repeater spesso si ometterà Web etichetta dei controlli a meno che non è possibile sapere che avrò bisogno di formattare l'aspetto dei dati associato testo in base a livello di codice per la logica.


[![Ogni prodotto s Output viene eseguito il rendering tramite DataList s ItemTemplate](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image19.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image18.png)

**Figura 8**: ogni prodotto s Output è eseguito il rendering tramite DataList s `ItemTemplate` ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image20.png))


## <a name="step-3-improving-the-appearance-of-the-datalist"></a>Passaggio 3: Migliorare l'aspetto del controllo DataList

Come per GridView, DataList offre una serie di proprietà correlate allo stile, ad esempio `Font`, `ForeColor`, `BackColor`, `CssClass`, `ItemStyle`, `AlternatingItemStyle`, `SelectedItemStyle`e così via. Quando si lavora con i controlli GridView e DetailsView, abbiamo creato i file di interfaccia nel `DataWebControls` tema predefiniti il `CssClass` le proprietà per questi due controlli e la `CssClass` proprietà per molti di loro sottoproprietà (`RowStyle` `HeaderStyle`e così via). Let s ripetere l'operazione per il controllo DataList.

Come descritto nel [visualizzazione di dati con ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) dell'esercitazione, un file di interfaccia specifica le proprietà correlate all'aspetto predefinito per un controllo Web; un tema è una raccolta di file dell'interfaccia personalizzata, CSS, immagini e JavaScript che definiscono un particolare aspetto di un sito Web. Nel *visualizzazione di dati con ObjectDataSource* esercitazione, è stato creato un `DataWebControls` tema (che viene implementato come una cartella all'interno di `App_Themes` cartella) con, attualmente, i file di interfaccia due - `GridView.skin` e `DetailsView.skin`. Consentire s aggiungere un terzo file di interfaccia per specificare le impostazioni di stile predefinito per il controllo DataList.

Per aggiungere un file di interfaccia, fare clic su di `App_Themes/DataWebControls` cartella, fare clic su Aggiungi un nuovo elemento e selezionare l'opzione soubor Skinu dall'elenco. Denominare il file `DataList.skin`.


[![Creare un nuovo File di interfaccia denominato DataList.skin](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image22.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image21.png)

**Figura 9**: creare un nuovo File di interfaccia denominato `DataList.skin` ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image23.png))


Usare il markup seguente per il `DataList.skin` file:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample4.aspx)]

Queste impostazioni assegnano le stesse classi CSS per le proprietà DataList appropriate come venivano utilizzati con i controlli GridView e DetailsView. Le classi CSS utilizzate in questo caso `DataWebControlStyle`, `AlternatingRowStyle`, `RowStyle`e così via sono definiti nel `Styles.css` file e sono state aggiunte nelle esercitazioni precedenti.

Con l'aggiunta di questo file di interfaccia, l'aspetto DataList viene aggiornato nella finestra di progettazione (potrebbe essere necessario aggiornare la visualizzazione della finestra di progettazione per visualizzare gli effetti del nuovo file di interfaccia; dal menu Visualizza, scegliere Aggiorna). Come illustrato nella figura 10, ogni prodotto alternativo ha un colore di sfondo di colore rosa chiaro.


[![Creare un nuovo File di interfaccia denominato DataList.skin](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image25.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image24.png)

**Figura 10**: creare un nuovo File di interfaccia denominato `DataList.skin` ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image26.png))


## <a name="step-4-exploring-the-datalist-s-other-templates"></a>Passaggio 4: Esplorazione DataList s altri modelli

Oltre al `ItemTemplate`, DataList supporta sei altri modelli facoltativi:

- `HeaderTemplate` Se fornito, aggiunge una riga di intestazione nell'output e viene usato per eseguire il rendering di questa riga
- `AlternatingItemTemplate` utilizzato per il rendering di elementi alternati
- `SelectedItemTemplate` utilizzato per il rendering dell'elemento selezionato; l'elemento selezionato è l'elemento il cui indice corrisponde a s DataList [ `SelectedIndex` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.selectedindex.aspx)
- `EditItemTemplate` utilizzato per il rendering dell'elemento in fase di modifica
- `SeparatorTemplate` Se fornito, aggiunge un separatore tra ciascun elemento e viene usato per eseguire il rendering di questo separatore
- `FooterTemplate` -Se fornito, aggiunge una riga di piè di pagina per l'output e viene usato per eseguire il rendering di questa riga

Quando si specifica la `HeaderTemplate` o `FooterTemplate`, DataList aggiunge una riga di intestazione o piè di pagina aggiuntiva per l'output sottoposto a rendering. Ad esempio con GridView s intestazioni e piè di pagina righe, l'intestazione e piè di pagina in un controllo DataList non associati a dati. Pertanto, qualsiasi sintassi di associazione dati nel `HeaderTemplate` o `FooterTemplate` che tenta di accedere ai dati associati restituirà una stringa vuota.

> [!NOTE]
> Come abbiamo visto nel [visualizzando le informazioni di riepilogo in GridView s piè di pagina](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs.md) esercitazione, mentre le righe di intestazione e piè di pagina si sintassi di associazione dati di supporto per t, le informazioni specifiche dei dati può essere inserito direttamente in queste righe dal S GridView `RowDataBound` gestore dell'evento. Questa tecnica può essere usata per entrambe calcolano i totali parziali o altri informazioni dai dati associata al controllo, nonché assegnare tali informazioni per il piè di pagina. Lo stesso concetto può essere applicato ai controlli DataList e Repeater; l'unica differenza è che per DataList e Repeater creare un gestore eventi per il `ItemDataBound` evento (invece che per il `RowDataBound` eventi).


Per questo esempio, ti permettono di s hanno il titolo del prodotto informazioni visualizzate nella parte superiore dei risultati in s DataList un `<h3>` intestazione. A tale scopo, aggiungere un `HeaderTemplate` con il markup appropriato. Dalla finestra di progettazione, è possibile fare clic sul collegamento di modifica modelli nello smart tag DataList s, la scelta del modello di intestazione nell'elenco a discesa e digitando nel testo dopo aver selezionato l'opzione di titolo 3 dallo stile elenco a discesa elenco (vedere la figura 11).


[![Aggiungere un modello HeaderTemplate con le informazioni sul prodotto di testo](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image28.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image27.png)

**Figura 11**: aggiungere un' `HeaderTemplate` con le informazioni sul prodotto di testo ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image29.png))


In alternativa, questa può essere aggiunta in modo dichiarativo tramite l'immissione di markup seguente all'interno di `<asp:DataList>` tag:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample5.html)]

Per aggiungere un po' di spazio tra ogni elenco dei prodotti, consentire s aggiungere un `SeparatorTemplate` che include una riga tra ciascuna sezione. Il tag righello orizzontale (`<hr>`), aggiunge un divisore di questo tipo. Creare il `SeparatorTemplate` in modo che includa il markup seguente:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample6.html)]

> [!NOTE]
> Ad esempio la `HeaderTemplate` e `FooterTemplates`, il `SeparatorTemplate` non è associato a tutti i record dall'origine dati e di conseguenza, non può direttamente accedere record associato a DataList dell'origine dati.


Dopo aver apportato questa aggiunta, la visualizzazione della pagina tramite un browser dovrebbe essere simile alla figura 12. Si noti la riga di intestazione e la riga tra ogni presentazione del prodotto.


[![DataList include una riga di intestazione e una regola orizzontale tra ogni presentazione del prodotto](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image31.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image30.png)

**Figura 12**: il controllo DataList include una riga di intestazione e un orizzontale regola tra ogni Product Listing ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image32.png))


## <a name="step-5-rendering-specific-markup-with-the-repeater-control"></a>Passaggio 5: Eseguire il Rendering di Markup specifico con il controllo Repeater

In caso una vista o origine dal browser quando si visita l'esempio di DataList dalla figura 12, si noterà che il controllo DataList genera un elemento HTML `<table>` che contiene una riga della tabella (`<tr>`) con una singola cella di tabella (`<td>`) per ogni elemento associato ai DataList. Questo output, infatti, è identico a ciò che potrebbe essere emessi da un controllo GridView con un singolo TemplateField. Come vedremo in un'esercitazione futura, DataList consentono ulteriore personalizzazione dell'output, ci permette di visualizzare più record di origine dati per ogni riga della tabella.

Cosa accade se non si vuole t per generare HTML `<table>`, anche se? Per il controllo totale e completato il markup generato da un controllo Web, è necessario usare il controllo Repeater. Ad esempio DataList, Repeater viene costruito basato su modelli. Il controllo Repeater, offre, tuttavia, solo i seguenti cinque modelli:

- `HeaderTemplate` Se specificato, viene aggiunto il markup specificato prima degli elementi
- `ItemTemplate` utilizzato per il rendering di elementi
- `AlternatingItemTemplate` Se specificato, utilizzato per il rendering di elementi alternati
- `SeparatorTemplate` Se specificato, viene aggiunto il markup specificato tra ogni elemento
- `FooterTemplate` -Se fornito, consente di aggiungere il markup specificato dopo gli elementi

In ASP.NET 1.x, Repeater controllo usate per visualizzare un elenco puntato con dati provengono da un'origine dati. In tal caso, il `HeaderTemplate` e `FooterTemplates` conterrà l'apertura e chiusura `<ul>` tag, rispettivamente, mentre il `ItemTemplate` conterrebbe `<li>` elementi con la sintassi di associazione dati. Questo approccio può ancora essere utilizzato in ASP.NET 2.0 come abbiamo visto nei due esempi nel [pagine Master e spostamento nel sito](../introduction/master-pages-and-site-navigation-cs.md) esercitazione:

- Nel `Site.master` pagina master, un controllo Repeater veniva utilizzato per visualizzare un elenco puntato del contenuto della mappa del sito di livello superiore (Basic Reporting, Filtering Reports, Customized Formatting e così via); annidati, un altro controllo Repeater veniva utilizzato per visualizzare le sezioni di elementi figlio del sezioni di primo livello
- In `SectionLevelTutorialListing.ascx`, un controllo Repeater veniva utilizzato per visualizzare un elenco puntato delle sezioni figli della sezione mappa del sito corrente

> [!NOTE]
> ASP.NET 2.0 viene introdotta la nuova [BulletedList controllo](https://msdn.microsoft.com/library/ms228101.aspx), che può essere associato a un controllo origine dati per visualizzare un elenco puntato semplice. Con il controllo BulletedList è non è necessario specificare alcun il codice HTML relativo all'elenco. al contrario, viene indicato semplicemente il campo dati da visualizzare come testo per ogni elemento dell'elenco.


Il controllo Repeater funge da un blocco catch tutti i dati di controllo Web. Se non esiste un controllo esistente che genera il markup necessario, è possibile utilizzare il controllo Repeater. Per illustrare l'utilizzo di Repeater, consentire s ottenuto l'elenco di categorie visualizzato sopra il controllo DataList informazioni prodotto creato nel passaggio 2. In particolare, ti permettono di s dispongono delle categorie visualizzate in HTML un sola riga `<table>` con ogni categoria visualizzata come una colonna nella tabella.

A tale scopo, avviare, trascinare un controllo Repeater dalla casella degli strumenti nella finestra di progettazione, sopra il controllo DataList informazioni di prodotto. Come con DataList e Repeater vengono inizialmente visualizzate come un riquadro grigio fino a quando non sono stati definiti i modelli.


[![Aggiungere un controllo Repeater per la finestra di progettazione](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image34.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image33.png)

**Figura 13**: aggiungere un controllo Repeater per la finestra di progettazione ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image35.png))


Una sola opzione di lì s nello smart tag Repeater s: Scegli origine dati. Scegliere di creare un nuovo oggetto ObjectDataSource e configurarlo per usare la `CategoriesBLL` classe s `GetCategories` (metodo).


[![Creare un nuovo oggetto ObjectDataSource](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image37.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image36.png)

**Figura 14**: creare un nuovo oggetto ObjectDataSource ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image38.png))


[![Configurare ObjectDataSource per usare la classe CategoriesBLL](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image40.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image39.png)

**Figura 15**: configurare ObjectDataSource per usare il `CategoriesBLL` classe ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image41.png))


[![Recuperare informazioni su tutte le categorie usando il metodo GetCategories](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image43.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image42.png)

**Figura 16**: recuperare informazioni su tutte le di categorie utilizzando il `GetCategories` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image44.png))


A differenza di DataList, Visual Studio non crea automaticamente un elemento ItemTemplate per il controllo Repeater dopo l'associazione a un'origine dati. Inoltre, i modelli di Repeater s non possono essere configurati tramite la finestra di progettazione e devono essere specificati in modo dichiarativo.

Per visualizzare le categorie come un'unica riga `<table>` con una colonna per ogni categoria, è necessario il Repeater per generare il markup simile al seguente:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample7.html)]

Poiché il `<td>Category X</td>` testo è quella che si ripete, queste informazioni verranno visualizzate nel Repeater s ItemTemplate. Il markup che viene visualizzata prima che - `<table><tr>` -disponibilità verrà inserita nel `HeaderTemplate` durante il markup finale - `</tr></table>` -verrà inserita nel `FooterTemplate`. Per immettere le impostazioni del modello, andare alla parte dichiarativa della pagina ASP.NET, fare clic sul pulsante di origine nell'angolo inferiore sinistro e digitare la sintassi seguente:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample8.aspx)]

Il controllo Repeater genera il markup come specificato dal relativi modelli, nient'altro, nulla meno preciso. Figura 17 mostra l'output di s Repeater quando viene visualizzato tramite un browser.


[![Una singola riga HTML &lt;tabella&gt; Elenca ogni categoria in una colonna separata](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image46.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image45.png)

**Figura 17**: una singola riga HTML `<table>` Elenca ogni categoria in una colonna separata ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image47.png))


## <a name="step-6-improving-the-appearance-of-the-repeater"></a>Passaggio 6: Migliorare l'aspetto del ripetitore

Poiché il controllo Repeater genera in modo preciso del markup specificato dai relativi modelli, deve accolta con sorpresa che non esistono proprietà correlate allo stile per il controllo Repeater. Per modificare l'aspetto del contenuto generato da Repeater, è necessario aggiungere manualmente il contenuto HTML o CSS necessario direttamente ai modelli di Repeater s.

Per questo esempio, consentire s sono le colonne di categoria alternano colori di sfondo, come con le righe alterne in DataList. A tale scopo, è necessario assegnare il `RowStyle` classe CSS da ogni elemento Repeater e il `AlternatingRowStyle` classe CSS da ogni elemento Repeater alternato tramite il `ItemTemplate` e `AlternatingItemTemplate` modelli, come illustrato di seguito:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample9.aspx)]

Let s anche aggiungere una riga di intestazione nell'output con il testo le categorie di prodotti. Poiché non abbiamo t sapere quante colonne nostro risultante `<table>` includerà di, è il modo più semplice per generare una riga di intestazione è garantita da coprire tutte le colonne da utilizzare *due* `<table>` s. Il primo `<table>` conterrà due righe della riga di intestazione e una riga che conterrà la singola riga, seconda `<table>` che include una colonna per ogni categoria nel sistema. Vale a dire, si desidera generare il markup seguente:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample10.html)]

Quanto segue `HeaderTemplate` e `FooterTemplate` comportare il markup desiderato:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample11.aspx)]

Figura 18 Mostra Repeater dopo aver apportate queste modifiche.


[![Le colonne di categoria alternativo nel colore di sfondo e include una riga di intestazione](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image49.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image48.png)

**Figura 18**: la categoria colonne alternativo nel colore di sfondo e include una riga di intestazione ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image50.png))


## <a name="summary"></a>Riepilogo

Mentre il controllo GridView rende più facile visualizzare, edit, delete, ordinamento e pagine di dati, l'aspetto è molto squadrati e griglia. Per un maggiore controllo sull'aspetto, è necessario impostare in modo da controlli DataList o Repeater. Entrambi questi controlli visualizzano un set di record usando i modelli anziché BoundField CheckBoxFields e così via.

DataList esegue il rendering come HTML `<table>` che, per impostazione predefinita, viene visualizzato ogni record dell'origine dati in una singola riga della tabella, come un controllo GridView con un TemplateField singolo. Come vedremo in un'esercitazione futura, tuttavia, il controllo DataList permettono più record da visualizzare per ogni riga della tabella. Il controllo Repeater, d'altra parte, rigorosamente genera del markup specificato nei propri modelli; non consente di aggiungere eventuale markup aggiuntivo e pertanto comunemente utilizzato per visualizzare i dati negli elementi HTML diverso da un `<table>` (ad esempio un elenco puntato).

Mentre con DataList e Repeater offrono maggiore flessibilità nell'output sottoposto a rendering, dispongono di molte delle funzionalità predefinite disponibili in GridView. Come verrà esaminato in esercitazioni future, alcune di queste funzionalità possono essere inseriti in senza troppi sforzi, ma si tenere presente che con DataList o Repeater anziché il controllo GridView limitare la funzionalità che è possibile usare senza dover implementare tali funzionalità l'utente corrente.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. I revisori per questa esercitazione sono state Yaakov Ellis, Liz Shulok, Randy Schmidt e Stacy Park. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [avanti](formatting-the-datalist-and-repeater-based-upon-data-cs.md)
