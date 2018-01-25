---
uid: web-forms/overview/data-access/paging-and-sorting/creating-a-customized-sorting-user-interface-cs
title: Creazione di un'interfaccia utente personalizzata di ordinamento (c#) | Documenti Microsoft
author: rick-anderson
description: "Quando la visualizzazione di un lungo elenco di dati ordinati, può essere molto utile per raggruppare i dati correlati introducendo righe separatore. In questa esercitazione si vedrà come le cre..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2006
ms.topic: article
ms.assetid: 6f81b633-9d01-4e52-ae4a-2ea6bc109475
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/creating-a-customized-sorting-user-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: dbd2f6c8f1e21529da8a0fbffab212a29f615cc1
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="creating-a-customized-sorting-user-interface-c"></a>Creazione di un'interfaccia utente personalizzata di ordinamento (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare App di esempio](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_27_CS.exe) o [Scarica il PDF](creating-a-customized-sorting-user-interface-cs/_static/datatutorial27cs1.pdf)

> Quando la visualizzazione di un lungo elenco di dati ordinati, può essere molto utile per raggruppare i dati correlati introducendo righe separatore. In questa esercitazione vedremo come creare questo tipo un'interfaccia utente di ordinamento.


## <a name="introduction"></a>Introduzione

Quando la visualizzazione di un lungo elenco di dati ordinati in cui sono presenti solo un numero limitato di valori nella colonna ordinata, un utente finale può risultare difficile distinguere dove esattamente, si verificano i limiti di differenza. Ad esempio, esistono 81 prodotti nel database di, ma solo nove scelte categoria diversa (otto categorie univoche più il `NULL` opzione). Si consideri il caso di un utente che è interessato a esaminare i prodotti che rientrano nella categoria frutti di mare. Da una pagina che elenca *tutti* dei prodotti in un singolo GridView, l'utente potrebbe decidere il migliore consiste nell'ordinare i risultati per categoria, che verrà raggruppati tutti i prodotti di frutti di mare insieme. Dopo l'ordinamento per categoria, l'utente deve quindi cercare nell'elenco, cercando in cui i prodotti raggruppati frutti di mare iniziare e terminare. Poiché i risultati vengono ordinati in ordine alfabetico per trovare i prodotti di frutti di mare il nome della categoria non è difficile, ma richiede comunque strettamente l'analisi dell'elenco di elementi nella griglia.

Per aiutare a evidenziare i limiti tra i gruppi ordinati, molti siti Web utilizzano un'interfaccia utente che aggiunge un separatore tra tali gruppi. Separatori come quella illustrata nella figura 1 consente a un utente più rapidamente trovare un gruppo specifico e identificare ai limiti, nonché verificare quali gruppi distinti esistono nei dati.


[![Ogni gruppo di categorie è chiaramente](creating-a-customized-sorting-user-interface-cs/_static/image2.png)](creating-a-customized-sorting-user-interface-cs/_static/image1.png)

**Figura 1**: ogni gruppo di categorie è chiaramente ([fare clic per visualizzare l'immagine ingrandita](creating-a-customized-sorting-user-interface-cs/_static/image3.png))


In questa esercitazione vedremo come creare questo tipo un'interfaccia utente di ordinamento.

## <a name="step-1-creating-a-standard-sortable-gridview"></a>Passaggio 1: Creazione di un controllo GridView Standard, ordinabile

Prima di è illustrato come aumentare GridView per fornire l'interfaccia di ordinamento avanzata, consente di creare innanzitutto un controllo GridView standard, ordinabile che s sono elencati i prodotti. Aprire il `CustomSortingUI.aspx` nella pagina di `PagingAndSorting` cartella. Aggiungere un controllo GridView alla pagina, impostare il relativo `ID` proprietà `ProductList`e associarlo a un nuovo oggetto ObjectDataSource. Configurare ObjectDataSource per utilizzare il `ProductsBLL` classe s `GetProducts()` metodo per la selezione di record.

Configurare quindi GridView in modo che contenga solo il `ProductName`, `CategoryName`, `SupplierName`, e `UnitPrice` BoundField ed elemento CheckBoxField non più disponibile. Infine, configurare il controllo GridView per supportare l'ordinamento selezionando la casella di controllo Abilita ordinamento nello smart tag GridView s (o impostando il relativo `AllowSorting` proprietà `true`). Dopo aver apportato queste aggiunte per la `CustomSortingUI.aspx` pagina dichiarativo dovrebbe essere simile al seguente:


[!code-aspx[Main](creating-a-customized-sorting-user-interface-cs/samples/sample1.aspx)]

Richiedere qualche istante per visualizzare l'avanzamento finora in un browser. Figura 2 mostra GridView ordinabile quando i dati viene ordinati in base alla categoria in ordine alfabetico.


[![S GridView ordinabile i dati vengono ordinati per categoria](creating-a-customized-sorting-user-interface-cs/_static/image5.png)](creating-a-customized-sorting-user-interface-cs/_static/image4.png)

**Figura 2**: s GridView ordinabile di dati vengono ordinati per categoria ([fare clic per visualizzare l'immagine ingrandita](creating-a-customized-sorting-user-interface-cs/_static/image6.png))


## <a name="step-2-exploring-techniques-for-adding-the-separator-rows"></a>Passaggio 2: Esplorazione di tecniche per l'aggiunta di righe separatore

Con generico, ordinabile GridView completato, è comunque in grado di aggiungere le righe separatore in GridView, prima di ogni gruppo ordinato univoco. Ma come possano tali righe inserite in GridView? In pratica, si base per scorrere le righe s GridView, determinare le differenze tra i valori nella colonna ordinata e quindi aggiungere la riga separatore appropriato. Quando si pensa a questo problema, sembra che la soluzione sia contenuta in s GridView naturale `RowDataBound` gestore dell'evento. Come accennato nel [formattazione basato su dati personalizzati](../custom-formatting/custom-formatting-based-upon-data-cs.md) dell'esercitazione, il gestore eventi viene comunemente utilizzato quando l'applicazione di formattazione a livello di riga in base ai dati di riga o le righe. Tuttavia, il `RowDataBound` gestore dell'evento non è la soluzione in questo caso, come le righe non è possibile aggiungere a GridView a livello di codice da questo gestore eventi. S GridView `Rows` insieme, in realtà è di sola lettura.

Per aggiungere ulteriori righe a GridView sono disponibili tre opzioni:

- Aggiungere queste righe di separatore di metadati per i dati effettivi che sono associati al controllo GridView.
- Dopo aver associato GridView ai dati, aggiungere ulteriori `TableRow` istanze al s GridView il controllo della raccolta
- Creare un controllo server personalizzato che estende il controllo GridView ed esegue l'override di tali metodi responsabile della costruzione della struttura s GridView

Creazione di un controllo server personalizzato sarebbe l'approccio migliore se questa funzionalità è stata necessaria in molte pagine web o in diversi siti Web. Tuttavia, comporti gran parte del codice e una completa l'esplorazione all'interno dei meccanismi interni s GridView. Pertanto, sarà non consideriamo questa opzione per questa esercitazione.

Gli altri due opzioni di aggiunta di righe per i dati effettivi da separatore associate a GridView e la modifica della raccolta di controllo GridView s dopo il relativo stato associato - attaccare il problema in modo diverso e meritano una discussione.

## <a name="adding-rows-to-the-data-bound-to-the-gridview"></a>Aggiunta di righe di dati associate a GridView.

Quando il controllo GridView è associato a un'origine dati, viene creato un `GridViewRow` per ogni record restituiti dall'origine dati. Pertanto, è possibile inserire le righe necessarie separatore aggiungendo record separatore all'origine dati prima di associarlo a GridView. Figura 3 viene illustrato questo concetto.


![Una tecnica prevede l'aggiunta di righe separatore all'origine dati](creating-a-customized-sorting-user-interface-cs/_static/image7.png)

**Figura 3**: una tecnica prevede l'aggiunta di righe separatore all'origine dati


Utilizzare i record di termine separatore racchiuso tra virgolette perché non sono presenti record separatore speciale; piuttosto, è necessario in qualche modo flag che un record specifico nell'origine dati viene utilizzato come un separatore, anziché una riga di dati normali. Per i nostri esempi, è re associazione un `ProductsDataTable` istanza a GridView, composto da `ProductRows`. È possibile contrassegnare un record come una riga del separatore impostando il relativo `CategoryID` proprietà `-1` (poiché tale valore può t esiste in genere).

Per utilizzare questa tecnica d è necessario eseguire la procedura seguente:

1. Recuperano i dati da associare a GridView (un `ProductsDataTable` istanza)
2. Ordinare i dati in base a GridView s `SortExpression` e `SortDirection` proprietà
3. Scorrere il `ProductsRows` nel `ProductsDataTable`ricercano in cui si trovano le differenze nella colonna ordinata
4. Il limite di ogni gruppo, inserire un record di separatore `ProductsRow` istanza nell'oggetto DataTable, uno che dispone di s `CategoryID` impostato su `-1` (o qualsiasi designazione è stata stabilita per contrassegnare un record come record separatore)
5. Dopo l'inserimento di righe separatore, associare a livello di programmazione i dati a GridView

Oltre a queste cinque passaggi, d è inoltre necessario fornire un gestore eventi per s GridView `RowDataBound` evento. In questo caso, è d consente di controllare ogni `DataRow` e determinare se è un separatore di riga, uno cui `CategoryID` è stato impostato `-1`. In questo caso, è d potrebbe essere necessario modificare la formattazione o il testo visualizzato nelle celle.

Utilizzando questa tecnica per inserire i limiti di gruppo ordinamento richiede leggermente più complesso rispetto a descritte in precedenza, che è necessario fornire anche un gestore eventi per s GridView `Sorting` evento e tenere traccia del `SortExpression` e `SortDirection` valori.

## <a name="manipulating-the-gridview-s-control-collection-after-it-s-been-databound"></a>La modifica di GridView s controllare la raccolta dopo di esso s stato associato a dati

Invece di messaggistica i dati prima di associarlo a GridView, è possibile aggiungere le righe separatore *dopo* i dati sono stati associati a GridView. Il processo di associazione di dati crea la gerarchia dei controlli di GridView s, che in realtà è semplicemente un `Table` istanza è composto da una raccolta di righe, ognuna delle quali è costituito da una raccolta di celle. In particolare, la raccolta di controllo GridView s contiene un `Table` oggetto alla radice, un `GridViewRow` (derivata dal `TableRow` classe) per ogni record il `DataSource` associato a GridView e un `TableCell` oggetto in ogni `GridViewRow` istanza per ogni campo dati di `DataSource`.

Per aggiungere righe separatore tra ogni gruppo di ordinamento, è possibile modificare direttamente questa gerarchia di controllo dopo che è stato creato. È possibile avere la certezza che la gerarchia del controllo GridView s è stata creata per l'ultima volta nel momento in cui che la pagina viene eseguito il rendering. Pertanto, esegue l'override di questo approccio il `Page` classe s `Render` (metodo), a quel punto della gerarchia di controllo finale s GridView viene aggiornata per includere le righe necessarie separatore. Figura 4 viene illustrato questo processo.


[![Una tecnica alternativa consente di modificare la gerarchia del controllo GridView s](creating-a-customized-sorting-user-interface-cs/_static/image9.png)](creating-a-customized-sorting-user-interface-cs/_static/image8.png)

**Figura 4**: una tecnica alternativa consente di modificare la gerarchia dei controlli di s GridView ([fare clic per visualizzare l'immagine ingrandita](creating-a-customized-sorting-user-interface-cs/_static/image10.png))


Per questa esercitazione, si userà questo approccio quest'ultimo per personalizzare l'esperienza utente di ordinamento.

> [!NOTE]
> Il codice è m presentate in questa esercitazione si basa sull'esempio fornito in [Teemu Keiski](http://aspadvice.com/blogs/joteke/default.aspx) post di blog s, [la riproduzione di un Bit con raggruppamento ordinamento GridView](http://aspadvice.com/blogs/joteke/archive/2006/02/11/15130.aspx).


## <a name="step-3-adding-the-separator-rows-to-the-gridview-s-control-hierarchy"></a>Passaggio 3: Aggiunta delle righe separatore per la gerarchia del controllo GridView s

Poiché si desidera solo aggiungere le righe separatore per la gerarchia del controllo GridView s dopo aver creato la gerarchia dei controlli e creato per l'ultima volta in visita la pagina, si desidera eseguire l'aggiunta alla fine del ciclo di vita di pagina, ma prima dell'effettiva c GridView gerarchia ontrollo è stato eseguito il rendering in HTML. L'ultimo punto possibili in corrispondenza del quale è possibile eseguire questa operazione è il `Page` classe s `Render` evento, è possibile eseguire l'override della classe di codice utilizzando la firma del metodo seguente:


[!code-csharp[Main](creating-a-customized-sorting-user-interface-cs/samples/sample2.cs)]

Quando il `Page` classe s originale `Render` metodo viene richiamato `base.Render(writer)` ognuno dei controlli nella pagina verrà visualizzato, generare il markup in base alla loro gerarchia di controllo. Pertanto, è fondamentale che entrambi chiamare `base.Render(writer)`, in modo che il rendering della pagina, e che si modifichi il s GridView controllo gerarchia prima di chiamare `base.Render(writer)`, in modo che le righe di separatori sono stati aggiunti per la gerarchia del controllo GridView s prima s stato eseguito il rendering.

Per inserire le intestazioni di gruppo di ordinamento è innanzitutto necessario assicurarsi che l'utente ha richiesto che i dati ordinati. Per impostazione predefinita, il contenuto di s GridView non è ordinato e pertanto non non è necessario immettere qualsiasi gruppo di intestazioni di ordinamento.

> [!NOTE]
> Se si desidera GridView in base a una determinata colonna quando la pagina viene caricata, chiamare il GridView `Sort` metodo nella prima visita pagina (ma non nei postback successivi). A tale scopo, aggiungere la chiamata nel `Page_Load` gestore dell'evento all'interno di un `if (!Page.IsPostBack)` condizionale. Fare riferimento al [Paging e ordinamento dei dati del Report](paging-and-sorting-report-data-cs.md) esercitazione informazioni per ulteriori informazioni sul `Sort` metodo.


Supponendo che i dati sono ordinati, l'attività successiva consiste nel determinare quale colonna è stata ordinati i dati e per analizzare le righe per le differenze nella colonna s valori. Il codice seguente assicura che i dati sono ordinati e consente di trovare la colonna da cui i dati sono ordinati:


[!code-csharp[Main](creating-a-customized-sorting-user-interface-cs/samples/sample3.cs)]

Se il controllo GridView ha ancora essere ordinato, s GridView `SortExpression` proprietà verrà non impostata. Pertanto, è solo necessario aggiungere le righe di separatore se questa proprietà ha un valore. In caso affermativo, è necessario quindi determinare l'indice della colonna di ordinamento dei dati. Questa operazione viene eseguita scorrendo s GridView `Columns` insieme, la ricerca della colonna il cui `SortExpression` proprietà è uguale a s GridView `SortExpression` proprietà. Oltre all'indice di colonna s, è inoltre acquisire il `HeaderText` proprietà, viene utilizzata quando si visualizzano le righe di separatore.

Con l'indice della colonna di ordinamento dei dati, il passaggio finale consiste nell'enumerare le righe di GridView. Per ogni riga è necessario determinare se il valore di colonna ordinata s è diverso dal valore di s colonna s ordinati riga precedente. Se in tal caso, è necessario inserire un nuovo `GridViewRow` istanza nella gerarchia di controllo. Questa operazione viene eseguita con il codice seguente:


[!code-csharp[Main](creating-a-customized-sorting-user-interface-cs/samples/sample4.cs)]

Questo codice inizia a livello di codice che fa riferimento il `Table` alla radice della gerarchia del controllo s GridView è stato trovato e la creazione di una variabile di stringa denominata `lastValue`. `lastValue`viene utilizzato per confrontare il valore di colonna ordinati s riga corrente con il valore s riga precedente. Successivamente, i dispositivi di GridView `Rows` raccolta viene enumerata e per ogni riga il valore della colonna ordinato viene memorizzato nel `currentValue` variabile.

> [!NOTE]
> Per determinare il valore della colonna s ordinati particolare riga utilizzare la cella s `Text` proprietà. Ciò vale per BoundField, ma non funzionerà nel modo desiderato per TemplateFields, CheckBoxFields e così via. Verrà esaminato come account per i campi di GridView alternativi a breve.


Il `currentValue` e `lastValue` variabili vengono quindi confrontate. Se sono diversi è necessario aggiungere una nuova riga del separatore per la gerarchia dei controlli. Questa operazione viene eseguita mediante la definizione dell'indice del `GridViewRow` nel `Table` oggetto s `Rows` insieme, la creazione di nuovi `GridViewRow` e `TableCell` istanze e quindi aggiungendo il `TableCell` e `GridViewRow` per il gerarchia del controllo.

Si noti che il separatore di riga s unico `TableCell` è formattato in modo che comprenda l'intera larghezza del controllo GridView, viene formattato con il `SortHeaderRowStyle` classe CSS e ha il `Text` nome della proprietà ad visualizzato sia del gruppo di ordinamento (ad esempio categoria) e il valore s del gruppo (ad esempio bevande). Infine, `lastValue` viene aggiornato il valore `currentValue`.

La classe CSS usata per formattare la riga di intestazione di gruppo ordinamento `SortHeaderRowStyle` deve essere specificata nel `Styles.css` file. È possibile utilizzare le impostazioni di stile contestare. Si usa il seguente:


[!code-css[Main](creating-a-customized-sorting-user-interface-cs/samples/sample5.css)]

Con il codice corrente, l'ordinamento interfaccia aggiunge intestazioni di gruppo di ordinamento durante l'ordinamento da qualsiasi BoundField (vedere Figura 5, che mostra una schermata durante l'ordinamento dal fornitore). Tuttavia, quando l'ordinamento in base a qualsiasi altro tipo di campo (ad esempio un CheckBoxField o TemplateField), le intestazioni di gruppo di ordinamento sono da trovare (vedere Figura 6).


[![L'interfaccia di ordinamento include le intestazioni di gruppo di ordinamento durante l'ordinamento da BoundField](creating-a-customized-sorting-user-interface-cs/_static/image12.png)](creating-a-customized-sorting-user-interface-cs/_static/image11.png)

**Figura 5**: l'ordinamento interfaccia include ordinamento gruppo intestazioni quando l'ordinamento da BoundField ([fare clic per visualizzare l'immagine ingrandita](creating-a-customized-sorting-user-interface-cs/_static/image13.png))


[![Le intestazioni di gruppo di ordinamento sono mancanti quando l'ordinamento un CheckBoxField](creating-a-customized-sorting-user-interface-cs/_static/image15.png)](creating-a-customized-sorting-user-interface-cs/_static/image14.png)

**Figura 6**: le intestazioni di gruppo di ordinamento sono mancanti quando l'ordinamento un CheckBoxField ([fare clic per visualizzare l'immagine ingrandita](creating-a-customized-sorting-user-interface-cs/_static/image16.png))


Mancano le intestazioni di gruppo di ordinamento durante l'ordinamento per un CheckBoxField, infatti, poiché il codice utilizza attualmente solo il `TableCell` s `Text` proprietà per determinare il valore della colonna di ordinamento per ogni riga. Per CheckBoxFields, il `TableCell` s `Text` proprietà è una stringa vuota, invece, il valore è disponibile tramite un controllo casella di controllo Web che si trova all'interno di `TableCell` s `Controls` insieme.

Per gestire i tipi di campo diversi da BoundField, è necessario aumentare il codice in cui il `currentValue` variabile viene assegnata a verificare l'esistenza di una casella di controllo nel `TableCell` s `Controls` insieme. Anziché utilizzare `currentValue = gvr.Cells[sortColumnIndex].Text`, sostituire il codice con quanto segue:


[!code-csharp[Main](creating-a-customized-sorting-user-interface-cs/samples/sample6.cs)]

Questo codice esamina la colonna ordinata `TableCell` per la riga corrente determinare se sono presenti in tutti i controlli di `Controls` insieme. Se sono presenti, e il primo controllo è una casella di controllo, il `currentValue` variabile è impostata su Sì o No, a seconda della casella di controllo s `Checked` proprietà. In caso contrario, il valore viene recuperato dal `TableCell` s `Text` proprietà. È possibile replicare la logica per gestire l'ordinamento per qualsiasi TemplateFields che potrebbero esistere in GridView.

Con l'aggiunta di codice precedente, le intestazioni di gruppo Ordina ora sono presenti quando si ordinano le elemento CheckBoxField non più supportate (vedere la figura 7).


[![Le intestazioni di gruppo di ordinamento sono ora presente quando l'ordinamento un CheckBoxField](creating-a-customized-sorting-user-interface-cs/_static/image18.png)](creating-a-customized-sorting-user-interface-cs/_static/image17.png)

**Figura 7**: le intestazioni di gruppo di ordinamento sono ora presente quando l'ordinamento un CheckBoxField ([fare clic per visualizzare l'immagine ingrandita](creating-a-customized-sorting-user-interface-cs/_static/image19.png))


> [!NOTE]
> Se si dispone di prodotti con `NULL` database i valori per il `CategoryID`, `SupplierID`, o `UnitPrice` campi, tali valori verranno visualizzati come stringhe vuote in GridView per impostazione predefinita, vale a dire il testo della riga s separatore per i prodotti con `NULL`leggerà i valori come categoria: (vale a dire s non esiste alcun nome dopo la categoria: come con categoria: bibite). Se si desidera un valore visualizzato qui è possibile impostare il BoundField [ `NullDisplayText` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.boundfield.nulldisplaytext.aspx) al testo che si desidera visualizzare oppure è possibile aggiungere un'istruzione condizionale nel metodo Render, quando si assegna il `currentValue` per il separatore riga s `Text` proprietà.


## <a name="summary"></a>Riepilogo

GridView non include molte opzioni predefinite per la personalizzazione dell'interfaccia di ordinamento. Tuttavia, con un frammento di codice di basso livello, è possibile modificare la gerarchia dei controlli s GridView per creare un'interfaccia più personalizzata s. In questa esercitazione è stato illustrato come aggiungere una riga di separatore di gruppo di ordinamento per un GridView ordinabile, che identifica in modo più semplice gruppi distinti e i limiti di tali gruppi. Per ulteriori esempi di interfacce di ordinamento personalizzate, estrarre [Scott Guthrie](https://weblogs.asp.net/scottgu/) s [ASP.NET 2.0 GridView ordinamento suggerimenti e consigli](https://weblogs.asp.net/scottgu/archive/2006/02/11/437995.aspx) post di blog.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

>[!div class="step-by-step"]
[Precedente](sorting-custom-paged-data-cs.md)
[Successivo](paging-and-sorting-report-data-vb.md)
