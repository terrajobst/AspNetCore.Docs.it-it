---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-cs
title: Master/dettagli filtro usando un controllo DropDownList (c#) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione viene illustrato come visualizzare i report master/dettaglio in una singola pagina web con controlli DropDownList per visualizzare i record 'master' e un controllo DataList per displ...
ms.author: aspnetcontent
ms.date: 07/18/2007
ms.assetid: 07fa47ae-e491-4a2f-b265-d342b9ddef46
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: ed2631e49786c81075099cca6941d98ba3b67e37
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818252"
---
<a name="masterdetail-filtering-with-a-dropdownlist-c"></a>Master/dettagli filtro usando un controllo DropDownList (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'App di esempio](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_33_CS.exe) o [Scarica il PDF](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/datatutorial33cs1.pdf)

> In questa esercitazione viene illustrato come visualizzare i report master o di dettaglio in un'unica pagina web mediante controlli DropDownList per visualizzare il record "master" e un controllo DataList da visualizzare "Dettagli".


## <a name="introduction"></a>Introduzione

Report master o di dettaglio, prima di tutto creata Usa un controllo GridView in precedenza [Master/Dettagli filtro con un controllo DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) inizio dell'esercitazione, illustrando alcuni set di record "master". L'utente può quindi il drill-down in uno dei record master, in tal modo la visualizzazione "dei dettagli." di record master Report master o di dettaglio sono la scelta ideale per la visualizzazione delle relazioni uno-a-molti e per visualizzare informazioni dettagliate dalle tabelle "wide" particolarmente (ovvero quelle che presenta un elevato numero di colonne). Abbiamo esplorato come implementare i report master/dettaglio tramite i controlli GridView e DetailsView nelle esercitazioni precedenti. In questa esercitazione e i successivi due, è possibile esaminare di nuovo questi concetti, ma lo stato attivo sull'uso di DataList e Repeater controlla invece.

In questa esercitazione verrà esaminato tramite un controllo DropDownList per contenere il record "master", con "Dettagli" vengono visualizzati i record in un controllo DataList.

## <a name="step-1-adding-the-masterdetail-tutorial-web-pages"></a>Passaggio 1: Aggiunta di pagine Web di esercitazione Master-Details

Prima di iniziare questa esercitazione, prima di tutto è opportuno aggiungere la cartella e le pagine ASP.NET, che sarà necessario per questa esercitazione e i successivi due trattano report master-Details mediante i controlli DataList e Repeater. Iniziare creando una nuova cartella nel progetto denominato `DataListRepeaterFiltering`. Successivamente, aggiungere le seguenti cinque pagine ASP.NET per questa cartella, con tutti gli elementi configurati per utilizzare la pagina master `Site.master`:

- `Default.aspx`
- `FilterByDropDownList.aspx`
- `CategoryListMaster.aspx`
- `ProductsForCategoryDetails.aspx`
- `CategoriesAndProducts.aspx`


![Creare una cartella DataListRepeaterFiltering e aggiungere le pagine ASP.NET dell'esercitazione](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image1.png)

**Figura 1**: creare un `DataListRepeaterFiltering` cartella e aggiungere le pagine ASP.NET dell'esercitazione


Successivamente, aprire il `Default.aspx` pagina e trascinare il `SectionLevelTutorialListing.ascx` controllo utente dal `UserControls` cartella nell'area di progettazione. Questo controllo utente, che è stato creato nel [pagine Master e spostamento nel sito](../introduction/master-pages-and-site-navigation-cs.md) esercitazione enumera la mappa del sito e consente di visualizzare le esercitazioni nella sezione corrente in un elenco puntato.


[![Aggiungere il controllo utente sectionleveltutoriallisting. ascx a default. aspx](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image3.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image2.png)

**Figura 2**: aggiungere i `SectionLevelTutorialListing.ascx` controllo utente da `Default.aspx` ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image4.png))


Per la visualizzazione elenco puntato le esercitazioni di master-details che verrà creato, è necessario aggiungerli alla mappa del sito. Aprire il `Web.sitemap` file e aggiungere il markup seguente dopo che il markup di nodo della mappa del sito "Visualizzazione di dati con il DataList e Repeater":

[!code-xml[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample1.xml)]


![Aggiornare la mappa del sito in modo da includere le nuove pagine ASP.NET](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image5.png)

**Figura 3**: aggiornare la mappa del sito in modo da includere le nuove pagine ASP.NET


## <a name="step-2-displaying-the-categories-in-a-dropdownlist"></a>Passaggio 2: Visualizzare le categorie in un controllo DropDownList

Report master o di dettaglio elencherà le categorie presenti in un elenco a discesa con i prodotti dell'elemento elenco selezionato visualizzati più in basso nella pagina in un controllo DataList. La prima attività prima di Stati Uniti, è quindi disporre le categorie visualizzate in un controllo DropDownList. Iniziare aprendo il `FilterByDropDownList.aspx` nella pagina di `DataListRepeaterFiltering` cartelle e trascinare un controllo DropDownList dalla casella degli strumenti nella finestra di progettazione della pagina. Successivamente, impostare il controllo DropDownList `ID` proprietà `Categories`. Fare clic sul collegamento dallo smart tag del controllo DropDownList Scegli origine dati e creare un nuovo oggetto ObjectDataSource denominato `CategoriesDataSource`.


[![Aggiungere un nuovo oggetto ObjectDataSource denominato CategoriesDataSource](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image7.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image6.png)

**Figura 4**: aggiungere un nuovo oggetto ObjectDataSource denominato `CategoriesDataSource` ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image8.png))


Configurare il nuovo oggetto ObjectDataSource in modo che richiama il `CategoriesBLL` della classe `GetCategories()` (metodo). Dopo la configurazione di ObjectDataSource è ancora necessario specificare quale campo dell'origine dati deve essere visualizzato in DropDownList e che uno deve essere associato come valore per ogni elemento dell'elenco. Dispone il `CategoryName` campo, come la visualizzazione e `CategoryID` come valore per ogni elemento dell'elenco.


[![Hanno la visualizzazione del controllo DropDownList il campo Nome categoria e CategoryID Usa come valore](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image10.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image9.png)

**Figura 5**: la visualizzazione DropDownList il `CategoryName` campo e l'utilizzo `CategoryID` come valore ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image11.png))


A questo punto si dispone di un controllo DropDownList popolata con i record dal `Categories` tabella (tutto eseguita in circa sei secondi). Figura 6 mostra lo stato di avanzamento fino ad ora, quando viene visualizzato tramite un browser.


[![Un elenco a discesa Elenca le categorie correnti](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image13.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image12.png)

**Figura 6**: un elenco a discesa Elenca le categorie corrente ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image14.png))


## <a name="step-2-adding-the-products-datalist"></a>Passaggio 2: Aggiunta di DataList prodotti

L'ultimo elemento nel report master o di dettaglio è elencare i prodotti associati alla categoria selezionata. A tale scopo, aggiungere un controllo DataList alla pagina e creare un nuovo oggetto ObjectDataSource denominato `ProductsByCategoryDataSource`. Disporre le `ProductsByCategoryDataSource` controllo recuperare i dati dal `ProductsBLL` della classe `GetProductsByCategoryID(categoryID)` (metodo). Poiché questo report master o di dettaglio è di sola lettura, scegliere il che opzione (nessuno) nelle schede INSERT, UPDATE e DELETE.


[![Selezionare il metodo GetProductsByCategoryID(categoryID)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image16.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image15.png)

**Figura 7**: selezionare il `GetProductsByCategoryID(categoryID)` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image17.png))


Dopo aver fatto clic su Avanti, la procedura guidata ObjectDataSource richiede per l'origine del valore per il `GetProductsByCategoryID(categoryID)` del metodo *`categoryID`* parametro. Per utilizzare il valore dell'oggetto selezionato `categories` DropDownList elemento impostato l'origine del parametro al controllo e il ControlID a `Categories`.


[![Impostare il parametro categoryID al valore di DropDownList categorie](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image19.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image18.png)

**Figura 8**: impostare il *`categoryID`* parametro sul valore della `Categories` DropDownList ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image20.png))


Dopo aver completato la procedura guidata Configura origine dati, Visual Studio genera automaticamente un `ItemTemplate` per il controllo DataList che visualizza il nome e il valore di ogni campo dati. È possibile migliorare il DataList per usare invece un' `ItemTemplate` che visualizza solo il nome del prodotto, categoria, fornitore, quantity per ogni unità e il prezzo insieme a un `SeparatorTemplate` che inserisce un `<hr>` elemento tra ogni elemento. Devo usare il `ItemTemplate` da un esempio nel [visualizzazione dei dati con i controlli DataList e Repeater](../displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-cs.md) esercitazione, ma è possibile usare qualsiasi markup del modello è trovare visivamente più interessante.

Dopo aver apportato queste modifiche, il DataList e markup relativo ObjectDataSource dovrebbe essere simile al seguente:

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample2.aspx)]

Si consiglia di estrarre lo stato di avanzamento in un browser. Durante la prima visita la pagina, vengono visualizzati i prodotti appartenenti alla categoria selezionata (bevande) (come illustrato nella figura 9), ma la modifica DropDownList non aggiorna i dati. Infatti, deve verificarsi un postback per il controllo DataList da aggiornare. A tale scopo è possibile imposta il controllo DropDownList `AutoPostBack` proprietà `true` o aggiungere un controllo pulsante Web alla pagina. Per questa esercitazione, si è ho optato per impostare il controllo DropDownList `AutoPostBack` proprietà `true`.

Le figure da 9 e 10 illustrano il rapporto master/dettaglio in azione.


[![Durante la prima visita la pagina, vengono visualizzati i prodotti di bevande](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image22.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image21.png)

**Figura 9**: durante la prima visita la pagina, vengono visualizzati i prodotti di bevande ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image23.png))


[![Se si seleziona automaticamente un nuovo prodotto (produzione), un PostBack, l'aggiornamento di DataList](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image25.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image24.png)

**Figura 10**: se si seleziona automaticamente un nuovo prodotto (produzione), un PostBack, l'aggiornamento di DataList ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image26.png))


## <a name="adding-a----choose-a-category----list-item"></a>Aggiunta di un elemento di elenco ", scegliere una categoria"

Quando si visita prima la `FilterByDropDownList.aspx` pagina le categorie primo elemento di elenco del DropDownList (bevande) è selezionata per impostazione predefinita, che mostra i prodotti di bevande in DataList. Nel *Master/Dettagli filtro con un controllo DropDownList* esercitazione abbiamo aggiunto un'opzione "-- seleziona una categoria," per il controllo DropDownList che è stato selezionato per impostazione predefinita e, quando selezionata, visualizzato *tutte* del prodotti nel database. Questo approccio è semplice da gestire quando si elencano i prodotti in un controllo GridView, come ogni riga del prodotto a occuparmi di una piccola quantità di superficie sullo schermo. Con il DataList, tuttavia, le informazioni di ogni prodotto utilizza una quantità eccessiva porzione più grande della schermata. È comunque possibile aggiungere un'opzione "-- seleziona una categoria," e averlo selezionata per impostazione predefinita, ma anziché mostrare tutti i prodotti quando è selezionata, è possibile configurarlo in modo che non mostra Nessun prodotto.

Per aggiungere un nuovo elemento elenco per il controllo DropDownList, passare alla finestra proprietà e fare clic sui puntini di sospensione di `Items` proprietà. Aggiungere un nuovo elemento elenco con il `Text` ", scegliere una categoria," e il `Value` `0`.


![Aggiungere un](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image27.png)

**Figura 11**: aggiungere un elemento di elenco ", scegliere una categoria"


In alternativa, è possibile aggiungere l'elemento dell'elenco, aggiungere il markup seguente per il controllo DropDownList:

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample3.aspx)]

Inoltre, è necessario impostare il controllo DropDownList `AppendDataBoundItems` al `true` poiché se è impostato su `false` (impostazione predefinita), quando le categorie vengono associate a DropDownList da ObjectDataSource, queste sovrascriveranno eventuali aggiunte manualmente dall'elenco elementi.


![Impostare la proprietà AppendDataBoundItems su True](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image28.png)

**Figura 12**: impostare il `AppendDataBoundItems` proprietà su True


Il motivo è stato scelto il valore `0` per l'elenco ", scegliere una categoria," elemento è perché sono presenti categorie nel sistema con valore `0`, di conseguenza non verrà restituito alcun record di prodotto quando è selezionata la voce di elenco ", scegliere una categoria,". Per risolvere il problema, si consiglia di visitare la pagina tramite un browser. Come illustrato nella figura 13, quando la visualizzazione della pagina viene selezionata la voce di elenco ", scegliere una categoria:" inizialmente e prodotti non vengono visualizzata.


[![Quando il](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image30.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image29.png)

**Figura 13**: quando è selezionata la voce di elenco ", scegliere una categoria,", i prodotti non vengono visualizzati ([fare clic per visualizzare l'immagine con dimensioni normali](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image31.png))


Se invece è visualizzerebbe *tutte* dei prodotti quando è selezionata l'opzione "-- seleziona una categoria,", usare il valore `-1` invece. Il lettore astuto ricorderà che riporta indietro nel *Master/Dettagli filtro con un controllo DropDownList* esercitazione abbiamo aggiornato il `ProductsBLL` della classe `GetProductsByCategoryID(categoryID)` metodo in modo che se un *`categoryID`* valore di `-1` è stata passata, tutto il prodotto di record restituiti.

## <a name="summary"></a>Riepilogo

Quando si visualizzano dati correlati in modo gerarchico, è spesso utile per presentare i dati usando i report master o di dettaglio, da cui l'utente può iniziare ad analizzare i dati nella parte superiore della gerarchia e il drill-down nei dettagli. In questa esercitazione abbiamo esaminato la creazione di un report master o di dettaglio che mostri i prodotti della categoria selezionata. Questa operazione è stata eseguita usando un controllo DropDownList per l'elenco di categorie e un controllo DataList per i prodotti appartenenti alla categoria selezionata.

Nella prossima esercitazione verrà esaminato separare i record master e i dettagli su due pagine. Nella prima pagina, un elenco di record "master" verrà visualizzato con un collegamento per visualizzare i dettagli. Facendo clic sul collegamento verrà whisk all'utente la seconda pagina, che verrà visualizzati i dettagli per il record principale selezionato.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali...

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore capo di questa esercitazione è stata Randy Schmidt. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [avanti](master-detail-filtering-acess-two-pages-datalist-cs.md)
