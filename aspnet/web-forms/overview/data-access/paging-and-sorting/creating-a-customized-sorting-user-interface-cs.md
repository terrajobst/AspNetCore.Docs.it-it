---
uid: web-forms/overview/data-access/paging-and-sorting/creating-a-customized-sorting-user-interface-cs
title: Creazione di un'interfaccia utente l'ordinamento personalizzato (c#) | Microsoft Docs
author: rick-anderson
description: Quando la visualizzazione di un lungo elenco di dati ordinati, può essere molto utile per raggruppare i dati correlati con l'introduzione di righe di separazione. In questa esercitazione verrà illustrato come cre...
ms.author: riande
ms.date: 08/15/2006
ms.assetid: 6f81b633-9d01-4e52-ae4a-2ea6bc109475
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/creating-a-customized-sorting-user-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: 28926276e183ae7cf7ed4087d8c73044741389e5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833371"
---
<a name="creating-a-customized-sorting-user-interface-c"></a>Creazione di un'interfaccia utente l'ordinamento personalizzato (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'App di esempio](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_27_CS.exe) o [Scarica il PDF](creating-a-customized-sorting-user-interface-cs/_static/datatutorial27cs1.pdf)

> Quando la visualizzazione di un lungo elenco di dati ordinati, può essere molto utile per raggruppare i dati correlati con l'introduzione di righe di separazione. In questa esercitazione vedremo come creare un'interfaccia utente ordinamento.


## <a name="introduction"></a>Introduzione

Quando la visualizzazione di un lungo elenco di dati ordinati in cui sono presenti solo un numero limitato di diversi valori nella colonna ordinata, un utente finale può risultare difficile distinguere in cui, esattamente, si verificano i limiti di differenza. Ad esempio, sono disponibili 81 prodotti nel database, ma solo nove scelte categoria diversa (otto categorie univoche più il `NULL` opzione). Si consideri il caso di un utente che è interessato a esaminare i prodotti che rientrano nella categoria frutti di mare. Da una pagina che elenca *tutti* dei prodotti in un singolo GridView, l'utente potrebbe decidere il modo migliore consiste nell'ordinare i risultati per categoria, che raggruppa insieme tutti i prodotti di frutti di mare insieme. Dopo l'ordinamento in base alla categoria, l'utente deve quindi cercare nell'elenco, cercando in cui i prodotti raggruppati frutti di mare iniziare e terminare. Poiché i risultati vengono ordinati in ordine alfabetico per nome della categoria di prodotti di frutti di mare di ricerca non è difficile, ma è comunque necessario strettamente l'analisi dell'elenco di elementi nella griglia.

Per aiutare a evidenziare i limiti tra i gruppi ordinati, molti siti Web utilizzano un'interfaccia utente che aggiunge un separatore tra tali gruppi. I separatori, ad esempio quelli illustrati nella figura 1 consente a un utente a più rapidamente trovare un gruppo specifico e identificare i suoi limiti, nonché verificare quali gruppi distinti esistono nei dati.


[![Ogni gruppo di categorie è chiaramente](creating-a-customized-sorting-user-interface-cs/_static/image2.png)](creating-a-customized-sorting-user-interface-cs/_static/image1.png)

**Figura 1**: ogni gruppo di categorie è chiaramente ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-customized-sorting-user-interface-cs/_static/image3.png))


In questa esercitazione vedremo come creare un'interfaccia utente ordinamento.

## <a name="step-1-creating-a-standard-sortable-gridview"></a>Passaggio 1: Creazione di un controllo GridView Standard e ordinabile

Prima viene analizzato come aumentare il controllo GridView per fornire l'interfaccia di ordinamento avanzato, consentire s prima di tutto creare un GridView standard, ordinabili in cui sono elencati i prodotti. Iniziare aprendo il `CustomSortingUI.aspx` nella pagina di `PagingAndSorting` cartella. Aggiungere un controllo GridView alla pagina, impostare relativi `ID` proprietà `ProductList`e associarlo a un nuovo oggetto ObjectDataSource. Configurare ObjectDataSource per usare la `ProductsBLL` classe s `GetProducts()` metodo per la selezione di record.

Successivamente, configurare il controllo GridView in modo che contenga solo le `ProductName`, `CategoryName`, `SupplierName`, e `UnitPrice` BoundField e le CampoCasellaDiControllo non più disponibile. Infine, configurare il controllo GridView per supportare l'ordinamento selezionando la casella di controllo Abilita ordinamento nello smart tag s GridView (oppure tramite l'impostazione relativa `AllowSorting` proprietà `true`). Dopo aver apportato queste aggiunte per il `CustomSortingUI.aspx` pagina dichiarativo dovrebbe essere simile al seguente:


[!code-aspx[Main](creating-a-customized-sorting-user-interface-cs/samples/sample1.aspx)]

Si consiglia di visualizzare lo stato di avanzamento fino a quel momento in un browser. Figura 2 mostra il controllo GridView ordinabile quando i dati viene ordinati in base alla categoria in ordine alfabetico.


[![Le s ordinabile GridView i dati sono ordinati per categoria](creating-a-customized-sorting-user-interface-cs/_static/image5.png)](creating-a-customized-sorting-user-interface-cs/_static/image4.png)

**Figura 2**: s GridView ordinabile dati vengono ordinati in base alla categoria ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-customized-sorting-user-interface-cs/_static/image6.png))


## <a name="step-2-exploring-techniques-for-adding-the-separator-rows"></a>Passaggio 2: Esplorare le tecniche per l'aggiunta di righe del separatore

Con generico, ordinabile GridView completo, resta che sia in grado di aggiungere le righe del separatore in GridView prima di ogni gruppo ordinato univoco. Ma come possano tali righe inserite in GridView? In pratica, è necessario per scorrere le righe s GridView, determinare dove si verificano le differenze tra i valori nella colonna ordinata e quindi aggiungere la riga del separatore appropriato. Quando si pensa a questo problema, può sembrare naturale che la soluzione si trova in una posizione in s GridView `RowDataBound` gestore dell'evento. Come accennato nel [formattazione basata su dati personalizzati](../custom-formatting/custom-formatting-based-upon-data-cs.md) dell'esercitazione, questo gestore eventi viene spesso utilizzato quando applicando la formattazione a livello di riga basati sui dati di riga o le righe. Tuttavia, il `RowDataBound` gestore dell'evento non è la soluzione in questo caso, man mano che le righe non possono essere aggiunti a GridView a livello di codice da questo gestore eventi. Le s GridView `Rows` insieme, in effetti, è di sola lettura.

Per aggiungere le righe aggiuntive per il controllo GridView sono disponibili tre opzioni:

- Aggiungere queste righe di separazione dei metadati per i dati effettivi che sono associati a GridView
- Dopo che il controllo GridView è stato associato ai dati, aggiungere altri `TableRow` istanze al s GridView controllano la raccolta
- Creare un controllo server personalizzato che estende il controllo GridView e sostituisce tali metodi responsabile della costruzione della struttura s GridView

Creazione di un controllo server personalizzato sarebbe l'approccio migliore se questa funzionalità è stata necessaria sul numero di pagine web o tra diversi siti Web. Tuttavia, comporti gran parte del codice e un'analisi completa le meraviglie i meccanismi interni di GridView s. Di conseguenza, abbiamo non sarà prendere in considerazione questa opzione per questa esercitazione.

Gli altri due le opzioni di aggiunta di righe separatore per i dati effettivi da associate a GridView. e la modifica della raccolta di controllo GridView s dopo il relativo stato associato: attaccare il problema in modo diverso e meritano una discussione.

## <a name="adding-rows-to-the-data-bound-to-the-gridview"></a>Aggiunta di righe a dati associate a GridView.

Quando il controllo GridView è associato a un'origine dati, viene creato un `GridViewRow` per ogni record restituito dall'origine dati. Pertanto, è possibile inserire le righe necessarie separatore aggiungendo separatore record nell'origine dati prima di associarlo a GridView. Figura 3 illustra questo concetto.


![Una tecnica prevede l'aggiunta di separatore righe nell'origine dati](creating-a-customized-sorting-user-interface-cs/_static/image7.png)

**Figura 3**: una tecnica prevede l'aggiunta di separatore righe nell'origine dati


Usare i record del termine separatore racchiuso tra virgolette perché non sono presenti record di separazione speciale; piuttosto, è necessario in qualche modo flag che un record specifico nell'origine dati viene usato come separatore anziché a una riga di dati normali. Per questi esempi si ri associazione un `ProductsDataTable` istanza per il controllo GridView, composto da `ProductRows`. È possibile contrassegnare un record come una riga del separatore impostando relativi `CategoryID` proprietà `-1` (perché tale valore non è stato t esiste in genere).

Per poter usare questa tecnica è il d necessari eseguire la procedura seguente:

1. Recuperano i dati da associare al controllo GridView (un `ProductsDataTable` istanza)
2. Ordinamento dei dati basati su s GridView `SortExpression` e `SortDirection` proprietà
3. Eseguire l'iterazione attraverso il `ProductsRows` nella `ProductsDataTable`ricercano in cui si trovano le differenze nella colonna ordinata
4. In ogni limite di gruppo, inserire un record di separatore `ProductsRow` istanza nel DataTable, che ne sia informata s `CategoryID` impostato su `-1` (o qualsiasi designazione è stato deciso di contrassegnare un record come record separatore)
5. Dopo aver inserito le righe di separazione, associare programmaticamente i dati a GridView

Oltre a queste cinque fasi, d dobbiamo anche specificare un gestore eventi per s GridView `RowDataBound` evento. In questo caso, è il d verificare ognuno `DataRow` e stabilire se questo è un separatore di riga, una cui `CategoryID` impostazione era `-1`. In questo caso, d probabilmente vogliamo modificare la relativa formattazione o il testo visualizzato nelle celle.

Utilizzando questa tecnica per inserire l'ordinamento ai gruppo di limiti richiede un po' più lavoro descritto in precedenza, perché è necessario fornire anche un gestore eventi per s GridView `Sorting` evento e tenere traccia del `SortExpression` e `SortDirection` valori.

## <a name="manipulating-the-gridview-s-control-collection-after-it-s-been-databound"></a>Modifica s GridView controllano la raccolta dopo di esso s stato associato a dati

Invece di messaggistica i dati prima di associarlo a GridView, possiamo aggiungere le righe di separazione *dopo* i dati sono stati associati a GridView. Il processo di data binding tende ad aumentare la gerarchia dei controlli GridView s, che in realtà è semplicemente un `Table` istanza è composto da una raccolta di righe, ognuna delle quali è costituito da una raccolta di celle. In particolare, la raccolta di controllo GridView s contiene un `Table` oggetto la cui radice, una `GridViewRow` (che deriva dal `TableRow` classe) per ciascun record nel `DataSource` associato a GridView e un `TableCell` oggetto in ogni `GridViewRow` per ogni campo dati nell'istanza di `DataSource`.

Per aggiungere righe separatore tra ciascun gruppo di ordinamento, è possibile gestire direttamente la gerarchia dei controlli dopo che è stato creato. Possiamo essere certi che sia stata creata la gerarchia dei controlli GridView s per l'ultima volta nel momento in cui che viene eseguito il rendering della pagina. Pertanto, esegue l'override di questo approccio il `Page` classe s `Render` metodo, a questo punto la gerarchia dei controlli finale s GridView viene aggiornata per includere le righe del separatore necessario. Figura 4 illustra questo processo.


[![Una tecnica alternativa consente di modificare la gerarchia dei controlli GridView s](creating-a-customized-sorting-user-interface-cs/_static/image9.png)](creating-a-customized-sorting-user-interface-cs/_static/image8.png)

**Figura 4**: una tecnica alternativa consente di modificare la gerarchia dei controlli di s GridView ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-customized-sorting-user-interface-cs/_static/image10.png))


Per questa esercitazione si userà questo secondo approccio per personalizzare l'esperienza utente di ordinamento.

> [!NOTE]
> Il codice si m presentando in questa esercitazione è basata sull'esempio fornito in [Teemu Keiski](http://aspadvice.com/blogs/joteke/default.aspx) intervento nel blog s [a cui è assegnato un Bit con il raggruppamento di ordinamento di GridView](http://aspadvice.com/blogs/joteke/archive/2006/02/11/15130.aspx).


## <a name="step-3-adding-the-separator-rows-to-the-gridview-s-control-hierarchy"></a>Passaggio 3: Aggiunta di righe di separazione per la gerarchia dei controlli GridView s

Poiché si vuole solo aggiungere le righe di separazione per la gerarchia dei controlli GridView s dopo aver creata la gerarchia dei controlli e creata per l'ultima volta in visita la pagina, è opportuno eseguire questa aggiunta alla fine del ciclo di vita di pagina, ma prima dell'effettiva c GridView gerarchia ontrollo è stato eseguito il rendering in HTML. L'ultimo punto possibile in corrispondenza del quale è possibile eseguire questa operazione è il `Page` classe s `Render` evento, che è possibile eseguire l'override nella classe code-behind uso della firma di metodo seguente:


[!code-csharp[Main](creating-a-customized-sorting-user-interface-cs/samples/sample2.cs)]

Quando la `Page` classe s originale `Render` metodo viene richiamato `base.Render(writer)` ognuno dei controlli nella pagina verrà visualizzato, generare il markup in base alla loro gerarchia di controllo. Pertanto è fondamentale che entrambi chiamiamo `base.Render(writer)`, in modo che il rendering della pagina, controllo e che si modifichi la s GridView gerarchia prima di chiamare `base.Render(writer)`, in modo che le righe di separazione sono stati aggiunti per la gerarchia dei controlli GridView s prima di esso s stato eseguito il rendering.

Per inserire le intestazioni di gruppo di ordinamento è necessario innanzitutto assicurarsi che l'utente ha richiesto che i dati da ordinare. Per impostazione predefinita, il contenuto di s GridView non è ordinato e pertanto temiamo t necessità di immettere qualsiasi gruppo di intestazioni di ordinamento.

> [!NOTE]
> Se si desidera che il controllo GridView in base a una determinata colonna quando la pagina viene caricata, chiamare il controllo GridView `Sort` metodo nella prima visita pagina (ma non nei postback successivi). A tale scopo, aggiungere questa chiamata nel `Page_Load` gestore dell'evento all'interno di un `if (!Page.IsPostBack)` condizionale. Fare riferimento al [Paging e ordinamento dei dati del Report](paging-and-sorting-report-data-cs.md) esercitazione informazioni per ulteriori informazioni sul `Sort` (metodo).


Supponendo che i dati sono stati ordinati, l'attività successiva consiste nel determinare quale colonna è stata ordinati i dati e quindi per analizzare le righe per le differenze in tale colonna s valori. Il codice seguente assicura che i dati sono stati ordinati e consente di trovare la colonna da cui i dati sono stati ordinati:


[!code-csharp[Main](creating-a-customized-sorting-user-interface-cs/samples/sample3.cs)]

Se il controllo GridView è ancora essere ordinati, la s GridView `SortExpression` proprietà sarà non siano stata impostata. Pertanto, si vuole solo aggiungere le righe del separatore se questa proprietà ha un valore. In caso affermativo, è necessario quindi determinare l'indice della colonna mediante il quale è stati ordinati i dati. Questa operazione viene eseguita eseguendo un ciclo s GridView `Columns` raccolta, la ricerca della colonna la cui proprietà `SortExpression` proprietà è uguale a s GridView `SortExpression` proprietà. Oltre all'indice della colonna s, si ottiene anche il `HeaderText` proprietà, che viene usata quando si visualizzano le righe del separatore.

Con l'indice della colonna mediante il quale i dati sono ordinati, il passaggio finale consiste nell'enumerare le righe di GridView. Per ogni riga è necessario determinare se il valore di colonna ordinata s è diverso dal valore di s colonna s ordinati riga precedente. Se è quindi necessario inserire un nuovo `GridViewRow` istanza nella gerarchia dei controlli. Questa operazione viene eseguita con il codice seguente:


[!code-csharp[Main](creating-a-customized-sorting-user-interface-cs/samples/sample4.cs)]

Inizia questo codice facendo riferimento a livello di codice le `Table` nella radice della gerarchia del controllo GridView s è stato trovato e la creazione di una variabile stringa denominata `lastValue`. `lastValue` Consente di confrontare il valore di colonna ordinati s riga corrente con il valore di riga o le righe precedenti. Successivamente, la s GridView `Rows` raccolta viene enumerata e per ogni riga il valore della colonna ordinata viene archiviato nel `currentValue` variabile.

> [!NOTE]
> Per determinare il valore della colonna particolare riga s ordinati utilizzerò la cella s `Text` proprietà. Ciò vale per BoundField, ma verrà non funziona nel modo desiderato per TemplateFields, CheckBoxFields e così via. Esamineremo come tener conto per i campi di GridView alternativi a breve.


Il `currentValue` e `lastValue` variabili vengono quindi confrontate. Se sono diversi è necessario aggiungere una nuova riga del separatore per la gerarchia dei controlli. Questa operazione viene eseguita determinando l'indice della `GridViewRow` nella `Table` oggetto s `Rows` raccolta, la creazione di nuove `GridViewRow` e `TableCell` istanze e l'aggiunta del `TableCell` e `GridViewRow` per il gerarchia dei controlli.

Si noti che il separatore di riga s contatteranno `TableCell` viene formattato in modo che comprenda l'intera larghezza del controllo GridView, viene formattato usando il `SortHeaderRowStyle` classe CSS e ha relativo `Text` ad che mostra sia il gruppo di ordinamento basato sul nome della proprietà (ad esempio, categoria) e il valore s del gruppo (ad esempio bibite). Infine `lastValue` viene aggiornato al valore di `currentValue`.

La classe CSS utilizzata per formattare la riga di intestazione gruppo ordinamento `SortHeaderRowStyle` deve essere specificato il `Styles.css` file. È possibile usare qualsiasi impostazione di stile contestare all'utente; Ho utilizzato il seguente:


[!code-css[Main](creating-a-customized-sorting-user-interface-cs/samples/sample5.css)]

Con il codice corrente, l'interfaccia ordinamento aggiunge intestazioni di gruppo ordinamento ordinare in base a qualsiasi BoundField (vedere la figura 5, che viene mostrata una schermata durante l'ordinamento dal fornitore). Tuttavia, quando l'ordinamento in base a qualsiasi altro tipo di campo (ad esempio un CampoCasellaDiControllo o TemplateField), le intestazioni di gruppo di ordinamento sono disponibile una destinazione da cercare (vedere la figura 6).


[![L'interfaccia ordinamento include intestazioni di gruppo di ordinamento durante l'ordinamento in base BoundField](creating-a-customized-sorting-user-interface-cs/_static/image12.png)](creating-a-customized-sorting-user-interface-cs/_static/image11.png)

**Figura 5**: l'ordinamento interfaccia include ordinamento gruppo intestazioni quando l'ordinamento in base BoundField ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-customized-sorting-user-interface-cs/_static/image13.png))


[![Le intestazioni di gruppo di ordinamento sono mancanti durante l'ordinamento un CampoCasellaDiControllo](creating-a-customized-sorting-user-interface-cs/_static/image15.png)](creating-a-customized-sorting-user-interface-cs/_static/image14.png)

**Figura 6**: le intestazioni di gruppo di ordinamento sono mancanti durante l'ordinamento un CampoCasellaDiControllo ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-customized-sorting-user-interface-cs/_static/image16.png))


Il motivo non sono presenti le intestazioni di gruppo di ordinamento durante l'ordinamento in base un CampoCasellaDiControllo è che il codice Usa attualmente solo il `TableCell` s `Text` proprietà per determinare il valore della colonna ordinata per ogni riga. Per CheckBoxFields, il `TableCell` s `Text` proprietà è una stringa vuota, invece, il valore è disponibile tramite un controllo casella di controllo Web che si trova all'interno di `TableCell` s `Controls` raccolta.

Per gestire i tipi di campo diverso da BoundField, è necessario aumentare il codice in cui il `currentValue` variabile è assegnata a verificare l'esistenza di una casella di controllo nel `TableCell` s `Controls` raccolta. Invece di usare `currentValue = gvr.Cells[sortColumnIndex].Text`, sostituire questo codice con quanto segue:


[!code-csharp[Main](creating-a-customized-sorting-user-interface-cs/samples/sample6.cs)]

Questo codice esamina la colonna ordinata `TableCell` per la riga corrente determinare se sono presenti tutti i controlli nel `Controls` raccolta. Se sono presenti, e il primo controllo è una casella di controllo, il `currentValue` variabile è impostata su Sì o No, a seconda della casella di controllo s `Checked` proprietà. In caso contrario, il valore da cui proviene il `TableCell` s `Text` proprietà. È possibile replicare questa logica per gestire l'ordinamento per qualsiasi TemplateFields che potrebbero essere presenti in GridView.

Con l'aggiunta di codice precedente, le intestazioni di gruppo Ordina ora sono presenti quando si ordinano le il CampoCasellaDiControllo non più disponibili (vedere la figura 7).


[![Le intestazioni di gruppo di ordinamento sono ora presenti durante l'ordinamento un CampoCasellaDiControllo](creating-a-customized-sorting-user-interface-cs/_static/image18.png)](creating-a-customized-sorting-user-interface-cs/_static/image17.png)

**Figura 7**: le intestazioni di gruppo di ordinamento sono ora presenti durante l'ordinamento un CampoCasellaDiControllo ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-customized-sorting-user-interface-cs/_static/image19.png))


> [!NOTE]
> Se si dispone di prodotti con `NULL` database i valori per il `CategoryID`, `SupplierID`, o `UnitPrice` , tali valori verranno visualizzati i campi come stringhe vuote in GridView per impostazione predefinita, vale a dire il testo riga s separatore per i prodotti con `NULL`leggeranno i valori come categoria: (vale a dire che vi s alcun nome dopo la categoria: come con categoria: bibite). Se si desidera che un valore visualizzato qui è possibile impostare i BoundField [ `NullDisplayText` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.boundfield.nulldisplaytext.aspx) per il testo da visualizzare oppure è possibile aggiungere un'istruzione condizionale nel metodo di rendering durante l'assegnazione di `currentValue` per il separatore riga s `Text` proprietà.


## <a name="summary"></a>Riepilogo

Il controllo GridView non include molte opzioni predefinite per la personalizzazione dell'interfaccia di ordinamento. Tuttavia, con un po' di codice di basso livello, è possibile modificare la gerarchia dei controlli GridView s per creare un'interfaccia più personalizzata. In questa esercitazione è stato illustrato come aggiungere una riga di separatore gruppo di ordinamento per un controllo GridView ordinabile, che identifica più facilmente i gruppi distinti e tali limiti di gruppi. Per altri esempi di interfacce di ordinamento personalizzati, consultare [Scott Guthrie](https://weblogs.asp.net/scottgu/) s [alcuni ASP.NET 2.0 GridView ordinamento suggerimenti e trucchi](https://weblogs.asp.net/scottgu/archive/2006/02/11/437995.aspx) post di blog.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Precedente](sorting-custom-paged-data-cs.md)
> [Successivo](paging-and-sorting-report-data-vb.md)
