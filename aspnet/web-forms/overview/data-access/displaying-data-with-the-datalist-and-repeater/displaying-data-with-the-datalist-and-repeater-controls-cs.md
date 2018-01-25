---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-cs
title: Visualizzazione di dati con DataList e Repeater (c#) | Documenti Microsoft
author: rick-anderson
description: "Nelle esercitazioni precedenti è stato utilizzato il controllo GridView per visualizzare i dati. A partire da questa esercitazione, si esamina la creazione di modelli di report comuni con..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: 0591cacc-b34b-4cf6-885e-2c9953bb0946
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 42203bdd7c22f3885eecab36dbd710d371107285
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="displaying-data-with-the-datalist-and-repeater-controls-c"></a>Visualizzazione di dati con DataList e Repeater (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare App di esempio](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_29_CS.exe) o [Scarica il PDF](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/datatutorial29cs1.pdf)

> Nelle esercitazioni precedenti è stato utilizzato il controllo GridView per visualizzare i dati. A partire da questa esercitazione, si esamina la creazione di modelli di report comuni con i controlli DataList e Repeater, a partire dalle nozioni di base di visualizzazione dei dati con questi controlli.


## <a name="introduction"></a>Introduzione

In tutti gli esempi in passato 28 esercitazioni, in caso di necessità visualizzare più record da un'origine dati è attivate per il controllo GridView. GridView viene eseguito il rendering di una riga per ogni record nell'origine dati, la visualizzazione di campi di record s dati nelle colonne. Sebbene GridView rende un gioco da ragazzi display, spostamento, ordinamento, modificare ed eliminare dati, l'aspetto è un po' squadrato. Inoltre, il codice responsabile per la struttura di s GridView è fissa include HTML `<table>` con una riga della tabella (`<tr>`) per ogni record e una cella della tabella (`<td>`) per ogni campo.

Per fornire un maggiore livello di personalizzazione dell'aspetto e il markup sottoposto a rendering per la visualizzazione di più record, ASP.NET 2.0 offre i controlli DataList e Repeater (che sono anche disponibili in ASP.NET versione 1. x). I controlli DataList e Repeater il rendering di contenuto mediante modelli anziché BoundField CheckBoxFields, ButtonFields e così via. Ad esempio GridView, DataList esegue il rendering come HTML `<table>`, ma consente i dati più record di origine da visualizzare per ogni riga della tabella. Ripetitore, d'altra parte, esegue il rendering di alcun markup aggiuntive rispetto a quella in modo esplicito specificare ed è un candidato ideale quando è necessario un controllo preciso il markup generato.

Tramite le esercitazioni successivo decine o in questo caso, vedremo durante la creazione di modelli di report comuni con i controlli DataList e Repeater, a partire dalle nozioni di base di visualizzazione dei dati con questi modelli di controlli. Vedremo come formattare questi controlli, come modificare il layout di record di origine dati in DataList, gli scenari comuni master/dettagli, modi per modificare ed eliminare dati, come spostarsi tra i record e così via.

## <a name="step-1-adding-the-datalist-and-repeater-tutorial-web-pages"></a>Passaggio 1: Aggiunta di DataList e pagine Web esercitazione Ripetitore

Prima di iniziare questa esercitazione, lasciare s innanzitutto è opportuno aggiungere le pagine ASP.NET che è necessario per questa esercitazione e le esercitazioni di alcuni Avanti occupa la visualizzazione dei dati utilizzando il DataList e Repeater. Iniziare creando una nuova cartella nel progetto denominato `DataListRepeaterBasics`. Successivamente, aggiungere le seguenti cinque pagine ASP.NET in questa cartella, con tutti gli elementi configurati per utilizzare la pagina master `Site.master`:

- `Default.aspx`
- `Basics.aspx`
- `Formatting.aspx`
- `RepeatColumnAndDirection.aspx`
- `NestedControls.aspx`


![Creare una cartella DataListRepeaterBasics e aggiungere le pagine ASP.NET dell'esercitazione](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image1.png)

**Figura 1**: creare un `DataListRepeaterBasics` cartella e aggiungere le pagine ASP.NET dell'esercitazione


Aprire il `Default.aspx` pagina e trascinare il `SectionLevelTutorialListing.ascx` controllo utente dal `UserControls` cartella nell'area di progettazione. Questo controllo utente, creata nel [pagine Master e esplorazione del sito](../introduction/master-pages-and-site-navigation-cs.md) esercitazione enumera la mappa del sito e visualizza le esercitazioni dalla sezione corrente in un elenco puntato.


[![Aggiungere il controllo utente SectionLevelTutorialListing.ascx Default.aspx](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image3.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image2.png)

**Figura 2**: aggiungere il `SectionLevelTutorialListing.ascx` controllo utente in `Default.aspx` ([fare clic per visualizzare l'immagine ingrandita](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image4.png))


Per visualizzare l'elenco puntato le esercitazioni di DataList e Repeater che verrà creata, è necessario aggiungerli alla mappa del sito. Aprire il `Web.sitemap` file e aggiungere il markup seguente dopo il tag di nodo all'aggiunta di pulsanti personalizzata mappa del sito:


[!code-xml[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample1.xml)]


![Aggiornare la mappa per includere le nuove pagine ASP.NET](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image5.png)

**Figura 3**: aggiornare la mappa per includere le nuove pagine ASP.NET


## <a name="step-2-displaying-product-information-with-the-datalist"></a>Passaggio 2: Visualizzare le informazioni di prodotto con DataList

Simile al controllo FormView, il controllo DataList s output sottoposto a rendering dipende modelli anziché BoundField, CheckBoxFields e così via. A differenza di FormView, DataList è progettato per visualizzare un set di record, anziché un unico metodo. Consente di iniziare questa esercitazione con informazioni di associazione a un controllo DataList s. Aprire il `Basics.aspx` nella pagina di `DataListRepeaterBasics` cartella. Successivamente, trascinare un controllo DataList dalla casella degli strumenti nella finestra di progettazione. Come illustrato nella figura 4, prima di specificare i modelli di DataList s, la finestra di progettazione viene visualizzato come casella grigia.


[![Trascinare il controllo DataList dalla casella degli strumenti nella finestra di progettazione](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image7.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image6.png)

**Figura 4**: trascinare il DataList dalla casella degli strumenti in Progettazione ([fare clic per visualizzare l'immagine ingrandita](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image8.png))


Da DataList s smart tag, aggiungere un nuovo oggetto ObjectDataSource e configurarlo per utilizzare il `ProductsBLL` classe s `GetProducts` metodo. Poiché è nuovamente la creazione di un controllo DataList di sola lettura in questa esercitazione, impostare l'elenco a discesa su (nessuno) in Creazione guidata s INSERT, aggiornare ed eliminare le schede.


[![Scegliere di creare un nuovo oggetto ObjectDataSource](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image10.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image9.png)

**Figura 5**: optare per creare un nuovo oggetto ObjectDataSource ([fare clic per visualizzare l'immagine ingrandita](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image11.png))


[![Configurare ObjectDataSource per utilizzare la classe ProductsBLL](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image13.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image12.png)

**Figura 6**: configurare ObjectDataSource per utilizzare il `ProductsBLL` classe ([fare clic per visualizzare l'immagine ingrandita](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image14.png))


[![Recuperare informazioni su tutti i prodotti con il metodo GetProducts](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image16.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image15.png)

**Figura 7**: recuperare informazioni su tutti di prodotti utilizzando il `GetProducts` metodo ([fare clic per visualizzare l'immagine ingrandita](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image17.png))


Dopo aver configurato il ObjectDataSource e associarla a DataList tramite smart tag, Visual Studio crea automaticamente un `ItemTemplate` in DataList che visualizza il nome e il valore di ogni campo di dati restituito dall'origine dati (vedere il markup riportato di seguito). Questa impostazione predefinita `ItemTemplate` aspetto s è identico a quello dei modelli creati automaticamente quando si associa un'origine dati al controllo FormView tramite la finestra di progettazione.


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample2.aspx)]

> [!NOTE]
> Tenere presente che quando si associa un'origine dati a un controllo FormView tramite lo smart tag di s FormView, Visual Studio ha creato un `ItemTemplate`, `InsertItemTemplate`, e `EditItemTemplate`. Con il controllo DataList, tuttavia, solo un `ItemTemplate` viene creato. Questo avviene perché DataList non dispongono della stessa modifica e l'inserimento di supporto offerto da FormView. DataList contengono eventi correlati a edit e delete, e la modifica ed eliminazione di supporto non consente di aggiungere un po' di codice, ma s non esiste alcun supporto della casella semplice, come con FormView. Vedremo come includere modifica ed eliminazione di supporto con DataList in un'esercitazione future.


Consente di dedicare alcuni minuti per migliorare l'aspetto del modello s. Anziché visualizzare tutti i campi di dati, consentire s consente di visualizzare solo il nome del prodotto s, fornitore, categoria, quantità per l'unità e il prezzo unitario. Inoltre, s consentono di visualizzare il nome in un `<h4>` intestazione e di disporre i campi rimanenti utilizzando un `<table>` sotto l'intestazione.

Per apportare queste modifiche, che è possibile utilizzare il funzionalità nella finestra di progettazione da DataList s smart tag fare clic sul collegamento Modifica modelli oppure è possibile modificare il modello manualmente tramite la sintassi dichiarativa s di pagina di modifica del modello. Se si utilizza l'opzione di modifica modelli nella finestra di progettazione, il tag risultante potrebbe non corrispondere esattamente il markup seguente, ma quando viene visualizzato tramite un browser sarà molto simile alla schermata illustrata nella figura 8.


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample3.aspx)]

> [!NOTE]
> Nell'esempio precedente viene controlli etichetta Web il cui `Text` proprietà viene assegnato il valore della sintassi di associazione dati. In alternativa, è stato possibile sono omessi le etichette, digitare solo la sintassi di associazione dati. Vale a dire, anziché utilizzare `<asp:Label ID="CategoryNameLabel" runat="server" Text='<%# Eval("CategoryName") %>' />` la sintassi dichiarativa avremmo potuto utilizzare invece `<%# Eval("CategoryName") %>`.


Lasciando nei controlli etichetta Web, tuttavia, offre due vantaggi. Innanzitutto fornisce un modo più semplice per la formattazione dei dati in base ai dati, come vedremo nella prossima esercitazione. In secondo luogo, l'opzione di modifica modelli nella sintassi di associazione dati dichiarativa visualizzazione di progettazione t che viene visualizzata all'esterno di un controllo Web. Al contrario, l'interfaccia di modifica modelli è progettato per facilitare l'utilizzo con markup statico e Web controlli si presuppone che qualsiasi associazione di dati verrà eseguita tramite la finestra di dialogo Modifica DataBindings accessibile dai controlli Web smart tag.

Pertanto, quando si lavora con DataList, che fornisce l'opzione di modifica dei modelli tramite la finestra di progettazione, è preferibile utilizzare i controlli Web di etichetta in modo che il contenuto è accessibile tramite l'interfaccia di modifica modelli. Come si vedrà a breve, Ripetitore richiede che il contenuto del modello s deve essere modificato dalla vista origine. Di conseguenza, durante la creazione di modelli di s Repeater si ometterà spesso Web etichetta controlli a meno che non è possibile sapere che sarà necessario formattare l'aspetto dei dati associato testo in base alla logica a livello di codice.


[![Ogni prodotto s Output viene eseguito il rendering tramite DataList s ItemTemplate](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image19.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image18.png)

**Figura 8**: ogni prodotto Output è sottoposto a rendering utilizzando DataList s `ItemTemplate` ([fare clic per visualizzare l'immagine ingrandita](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image20.png))


## <a name="step-3-improving-the-appearance-of-the-datalist"></a>Passaggio 3: Migliorare l'aspetto del controllo DataList

Ad esempio GridView, DataList offre una serie di proprietà correlate allo stile, ad esempio `Font`, `ForeColor`, `BackColor`, `CssClass`, `ItemStyle`, `AlternatingItemStyle`, `SelectedItemStyle`e così via. Quando si lavora con i controlli GridView e DetailsView, abbiamo creato il file di interfaccia nel `DataWebControls` tema predefiniti di `CssClass` le proprietà di questi due controlli e `CssClass` proprietà per diversi le sottoproprietà (`RowStyle` `HeaderStyle`e così via). Consente di eseguire la stessa operazione per il controllo DataList s.

Come descritto nel [la visualizzazione di dati con ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) dell'esercitazione, un file di interfaccia specifica le proprietà correlate all'aspetto predefinito per un controllo Web; un tema è una raccolta di file JavaScript, CSS, immagini e interfaccia che definiscono un particolare aspetto di un sito Web. Nel *la visualizzazione di dati con ObjectDataSource* esercitazione, è stato creato un `DataWebControls` tema (che viene implementato come una cartella all'interno di `App_Themes` cartella) che include, attualmente, due file di interfaccia - `GridView.skin` e `DetailsView.skin`. Consente di aggiungere un terzo file di interfaccia per specificare le impostazioni di stile predefinito per il controllo DataList s.

Per aggiungere un file di interfaccia, fare clic su di `App_Themes/DataWebControls` cartella, scegliere Aggiungi un nuovo elemento e selezionare l'opzione File di interfaccia dall'elenco. Denominare il file `DataList.skin`.


[![Creare un nuovo File di interfaccia denominato DataList.skin](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image22.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image21.png)

**Figura 9**: creare un nuovo File di interfaccia denominato `DataList.skin` ([fare clic per visualizzare l'immagine ingrandita](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image23.png))


Utilizzare il seguente codice per il `DataList.skin` file:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample4.aspx)]

Queste impostazioni assegnano le stesse classi CSS per le proprietà DataList appropriate sono state utilizzate con i controlli GridView e DetailsView. Le classi CSS utilizzate qui `DataWebControlStyle`, `AlternatingRowStyle`, `RowStyle`e così via sono definiti nel `Styles.css` file e sono stati aggiunti nelle esercitazioni precedenti.

Con l'aggiunta di questo file di interfaccia, l'aspetto di DataList s è stata aggiornata nella finestra di progettazione (potrebbe essere necessario aggiornare la visualizzazione della finestra di progettazione per visualizzare gli effetti del nuovo file di interfaccia; dal menu Visualizza, scegliere Aggiorna). Come mostrato nella figura 10, ogni prodotto alternativo dispone di un colore di sfondo di colore rosa chiaro.


[![Creare un nuovo File di interfaccia denominato DataList.skin](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image25.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image24.png)

**Figura 10**: creare un nuovo File di interfaccia denominato `DataList.skin` ([fare clic per visualizzare l'immagine ingrandita](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image26.png))


## <a name="step-4-exploring-the-datalist-s-other-templates"></a>Passaggio 4: Esplorazione DataList s altri modelli

Oltre al `ItemTemplate`, DataList supporta sei altri modelli facoltativi:

- `HeaderTemplate`Se fornito, aggiunge all'output una riga di intestazione e viene utilizzato per eseguire il rendering di questa riga
- `AlternatingItemTemplate`usato per il rendering di elementi alternativi
- `SelectedItemTemplate`utilizzato per il rendering dell'elemento selezionato; l'elemento selezionato è l'elemento il cui indice corrisponde a DataList s [ `SelectedIndex` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.selectedindex.aspx)
- `EditItemTemplate`utilizzato per il rendering dell'elemento da modificare
- `SeparatorTemplate`Se fornito, aggiunge un separatore tra ogni elemento e viene utilizzato per eseguire il rendering di questo tipo di separatore
- `FooterTemplate`-Se fornito, aggiunge all'output una riga di piè di pagina e viene utilizzato per eseguire il rendering di questa riga

Quando si specifica il `HeaderTemplate` o `FooterTemplate`, DataList aggiunge una riga di intestazione o piè di pagina aggiuntiva per l'output sottoposto a rendering. Ad esempio con GridView s intestazioni e piè di pagina righe, l'intestazione e piè di pagina in un controllo DataList non associati a dati. Pertanto, l'associazione dati sintassi di `HeaderTemplate` o `FooterTemplate` che tenta di accedere ai dati associati restituirà una stringa vuota.

> [!NOTE]
> Come illustrato nel [la visualizzazione di informazioni di riepilogo in GridView s piè di pagina](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs.md) esercitazione, mentre le righe di intestazione e piè di pagina si sintassi di associazione dati di supporto t, le informazioni relative ai dati può essere inserite direttamente in queste righe dal S GridView `RowDataBound` gestore dell'evento. Questa tecnica può essere utilizzata per entrambi calcolano i totali parziali o altre informazioni dai dati associata al controllo, nonché assegnare tali informazioni per il piè di pagina. Lo stesso concetto può essere applicato per i controlli DataList e Repeater. l'unica differenza è che per il DataList e Repeater creare un gestore eventi per il `ItemDataBound` evento (invece che per il `RowDataBound` evento).


Per questo esempio, consentono di s è il titolo di informazioni sul prodotto visualizzato nella parte superiore dei risultati s DataList in un `<h3>` titolo. A tale scopo, aggiungere un `HeaderTemplate` con il markup appropriato. Dalla finestra di progettazione, questa operazione può essere eseguita facendo clic sul collegamento Modifica modelli nello smart tag DataList s, la scelta del modello di intestazione nell'elenco a discesa e digitando del testo dopo aver selezionato l'opzione titolo 3 dall'elenco a discesa lo stile elenco (vedere Figura 11).


[![Aggiungere un HeaderTemplate con le informazioni sul prodotto di testo](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image28.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image27.png)

**Figura 11**: aggiungere un `HeaderTemplate` con le informazioni sul prodotto di testo ([fare clic per visualizzare l'immagine ingrandita](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image29.png))


In alternativa, può essere aggiunto in modo dichiarativo immettendo il seguente codice all'interno di `<asp:DataList>` tag:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample5.html)]

Per aggiungere un bit di spazio tra ogni elenco di prodotti, consentire s di aggiungere un `SeparatorTemplate` che include una riga tra ogni sezione. Il tag di riga orizzontale (`<hr>`), aggiunge un divisore di questo tipo. Creare il `SeparatorTemplate` in modo che abbia il markup seguente:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample6.html)]

> [!NOTE]
> Ad esempio il `HeaderTemplate` e `FooterTemplates`, `SeparatorTemplate` non è associato a qualsiasi record dall'origine dati e pertanto non può essere direttamente l'origine dati associati a DataList i record di accesso.


Dopo aver apportato questa aggiunta, la visualizzazione della pagina tramite un browser dovrebbe essere simile alla figura 12. Si noti la riga di intestazione e la riga tra ogni elenco di prodotti.


[![DataList include una riga di intestazione e una regola orizzontale tra ogni elenco di prodotti](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image31.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image30.png)

**Figura 12**: DataList include una riga di intestazione e un orizzontale regola tra ogni Product Listing ([fare clic per visualizzare l'immagine ingrandita](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image32.png))


## <a name="step-5-rendering-specific-markup-with-the-repeater-control"></a>Passaggio 5: Eseguire il Rendering di Markup specifico con il controllo Repeater

Caso un'origine dal browser durante la visita di esempio DataList dalla figura 12, si noterà che il controllo DataList crea un elemento HTML `<table>` che contiene una riga della tabella (`<tr>`) con una singola cella di tabella (`<td>`) per ogni elemento associato al DataList. In realtà, questo output è identico a ciò che potrebbe essere emessi da un GridView con un singolo TemplateField. Come si vedrà in un'esercitazione future, DataList consentono ulteriore personalizzazione dell'output, consentono di visualizzare più record di origine dati per ogni riga della tabella.

Cosa accade se si desidera creare un elemento HTML t `<table>`, anche se? Per il controllo totale e completato il markup generato da un controllo Web, è necessario utilizzare il controllo Repeater. Ad esempio DataList, Repeater viene costruito basato su modelli. Ripetitore, tuttavia, offre solo cinque modelli seguenti:

- `HeaderTemplate`Se specificato, viene aggiunto il markup specificato prima gli elementi
- `ItemTemplate`usato per il rendering di elementi
- `AlternatingItemTemplate`Se specificato, utilizzato per eseguire il rendering degli elementi alternati
- `SeparatorTemplate`Se fornito, viene aggiunto il markup specificato tra ogni elemento
- `FooterTemplate`-Se fornito, aggiunge il tag specificato dopo gli elementi

In ASP.NET 1. x, Ripetitore controllo usate per visualizzare un elenco puntato i cui dati provengono da un'origine dati. In tal caso, il `HeaderTemplate` e `FooterTemplates` conterrà l'apertura e chiusura `<ul>` tag, rispettivamente, mentre il `ItemTemplate` conterrebbe `<li>` elementi con la sintassi di associazione dati. Questo approccio può comunque essere utilizzato in ASP.NET 2.0 come abbiamo visto nei due esempi nel [pagine Master e esplorazione del sito](../introduction/master-pages-and-site-navigation-cs.md) esercitazione:

- Nel `Site.master` pagina master, un ripetitore utilizzato per visualizzare un elenco puntato del contenuto della mappa del sito di livello superiore (Reporting di base, il filtro di report, personalizzare la formattazione e così via); Ripetitore a un altro, nidificata utilizzata per visualizzare le sezioni di elementi figlio del sezioni di primo livello
- In `SectionLevelTutorialListing.ascx`, un ripetitore utilizzato per visualizzare un elenco puntato delle sezioni della sezione corrente della mappa del sito figlio

> [!NOTE]
> ASP.NET 2.0 viene introdotta la nuova [controllo BulletedList](https://msdn.microsoft.com/library/ms228101.aspx), che può essere associato a un controllo origine dati per visualizzare un elenco puntato semplice. Con il controllo di BulletedList non è necessario specificare uno qualsiasi del codice HTML relativo all'elenco. al contrario, si indica semplicemente il campo dei dati da visualizzare come il testo per ogni elemento nell'elenco.


Ripetitore funge da un blocco catch tutti i dati di controllo Web. Se non è un controllo esistente che genera il markup necessario, è possibile utilizzare il controllo Repeater. Per illustrare l'utilizzo di Repeater, consentire s dispone dell'elenco delle categorie visualizzato di sopra di DataList informazioni prodotto creato nel passaggio 2. In particolare, consentono di s hanno le categorie visualizzate in HTML un sola riga `<table>` con ogni categoria visualizzato come una colonna nella tabella.

A tale scopo, avviare mediante il trascinamento dalla casella degli strumenti nella finestra di progettazione sopra DataList informazioni prodotto di un controllo Repeater. Come con DataList, Repeater vengono inizialmente visualizzate come una casella di colore grigio fino a quando non sono stati definiti i modelli.


[![Aggiungere un controllo Repeater nella finestra di progettazione](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image34.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image33.png)

**Figura 13**: aggiungere un controllo Repeater nella finestra di progettazione ([fare clic per visualizzare l'immagine ingrandita](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image35.png))


Solo una delle opzioni qui s nello smart tag Ripetitore s: Scegli origine dati. Scegliere di creare un nuovo oggetto ObjectDataSource e configurarlo per utilizzare il `CategoriesBLL` classe s `GetCategories` metodo.


[![Creare un nuovo oggetto ObjectDataSource](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image37.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image36.png)

**Nella figura 14**: creare un nuovo oggetto ObjectDataSource ([fare clic per visualizzare l'immagine ingrandita](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image38.png))


[![Configurare ObjectDataSource per utilizzare la classe CategoriesBLL](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image40.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image39.png)

**Figura 15**: configurare ObjectDataSource per utilizzare il `CategoriesBLL` classe ([fare clic per visualizzare l'immagine ingrandita](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image41.png))


[![Recuperare informazioni su tutte le categorie utilizzando il metodo GetCategories](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image43.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image42.png)

**Figura 16**: recuperare informazioni su tutte le categorie utilizzando il `GetCategories` metodo ([fare clic per visualizzare l'immagine ingrandita](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image44.png))


A differenza di DataList, Visual Studio non crea automaticamente un elemento ItemTemplate per Ripetitore dopo l'associazione a un'origine dati. Inoltre, i modelli di s Ripetitore non possono essere configurati tramite la finestra di progettazione e devono essere specificati in modo dichiarativo.

Per visualizzare le categorie come un'unica riga `<table>` con una colonna per ogni categoria, è necessario Ripetitore per generare il markup simile al seguente:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample7.html)]

Poiché il `<td>Category X</td>` testo è quella che si ripete, questo nome verrà visualizzato nel Repeater s ItemTemplate. Il markup che viene visualizzato prima - `<table><tr>` -verrà inserita nel `HeaderTemplate` durante il markup finale - `</tr></table>` -verrà inserita nel `FooterTemplate`. Per immettere le impostazioni del modello, vedere la parte della pagina ASP.NET dichiarativa facendo clic sul pulsante di origine nell'angolo inferiore sinistro e il tipo nella sintassi seguente:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample8.aspx)]

Ripetitore genera il markup come specificato per i modelli, semplicemente, non meno preciso. Figura 17 mostra l'output di s Ripetitore quando viene visualizzato tramite un browser.


[![Una singola riga di HTML &lt;tabella&gt; Elenca ogni categoria in una colonna separata](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image46.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image45.png)

**Figura 17**: A riga singola HTML `<table>` Elenca ogni categoria in una colonna separata ([fare clic per visualizzare l'immagine ingrandita](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image47.png))


## <a name="step-6-improving-the-appearance-of-the-repeater"></a>Passaggio 6: Migliorare l'aspetto del ripetitore

Poiché il controllo Repeater genera con precisione il markup specificato dai relativi modelli, è necessario sorpresa che non sono disponibili proprietà correlate allo stile per il controllo Repeater. Per modificare l'aspetto del contenuto generato dal Repeater, è necessario aggiungere manualmente il contenuto HTML o CSS necessario direttamente ai modelli Ripetitore s.

Per questo esempio, consentire s sono le colonne di categoria alternativo colori di sfondo, ad esempio con le righe alterne in DataList. A tale scopo, è necessario assegnare il `RowStyle` classe CSS per ogni elemento Repeater e `AlternatingRowStyle` classe CSS per ogni elemento Repeater alternato tramite il `ItemTemplate` e `AlternatingItemTemplate` modelli, come illustrato di seguito:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample9.aspx)]

Consentire s inoltre aggiungere una riga di intestazione per l'output con il testo di categorie di prodotti. Poiché non abbiamo t sapere quante colonne nostri risultante `<table>` verrà integrata in, il modo più semplice per generare una riga di intestazione che è garantita da coprire tutte le colonne è utilizzare *due* `<table>` s. Il primo `<table>` conterrà due righe, la riga di intestazione e una riga che conterrà la singola riga, seconda `<table>` che include una colonna per ogni categoria nel sistema. Ovvero, si desidera generare il markup seguente:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample10.html)]

Nell'esempio `HeaderTemplate` e `FooterTemplate` generare il codice desiderato:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample11.aspx)]

Figura 18 Mostra Ripetitore dopo avere apportate queste modifiche.


[![Le colonne di categoria alternano nel colore di sfondo e include una riga di intestazione](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image49.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image48.png)

**Figura 18**: la categoria colonne alternativo nel colore di sfondo e include una riga di intestazione ([fare clic per visualizzare l'immagine ingrandita](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image50.png))


## <a name="summary"></a>Riepilogo

Mentre il controllo GridView consente di semplificare la visualizzazione, modifica, eliminazione, ordinamento e pagine di dati, l'aspetto è molto squadrati e griglia. Per un maggiore controllo sull'aspetto, è necessario attivare ai controlli DataList o Repeater. Entrambi i controlli visualizzare un set di record mediante modelli anziché BoundField CheckBoxFields e così via.

DataList esegue il rendering come HTML `<table>` che, per impostazione predefinita, viene visualizzato ciascun record di origine dati in una riga nella tabella, come un GridView con un singolo TemplateField. Come verrà illustrato in un'esercitazione futura, tuttavia, DataList consentire più record da visualizzare per ogni riga della tabella. Ripetitore, d'altra parte, strettamente genera il markup specificato nei propri modelli; non aggiunge alcun markup aggiuntivi e pertanto viene comunemente utilizzata per visualizzare i dati negli elementi HTML diverso da un `<table>` (ad esempio un elenco puntato).

Mentre il DataList e Repeater offrono maggiore flessibilità nell'output sottoposto a rendering, dispongono di molte delle funzionalità incorporate in GridView. Come verranno esaminati in esercitazioni future, alcune di queste funzionalità possono essere collegate nuovamente senza troppo complessa, ma si tenere presente che con DataList o Repeater utilizzabile GridView limitare le funzionalità che è possibile utilizzare senza dover implementare tali funzionalità l'utente corrente.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Lead revisori per questa esercitazione sono stati Yaakov Ellis, Liz Shulok, Randy Schmidt e Stacy Park. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[avanti](formatting-the-datalist-and-repeater-based-upon-data-cs.md)
