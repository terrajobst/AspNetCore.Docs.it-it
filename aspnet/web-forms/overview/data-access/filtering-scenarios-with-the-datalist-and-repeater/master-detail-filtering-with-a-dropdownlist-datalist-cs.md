---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-cs
title: Master-Details filtri con un controllo DropDownList (c#) | Documenti Microsoft
author: rick-anderson
description: In questa esercitazione viene illustrato come visualizzare i report master/dettaglio in una singola pagina web usando i controlli DropDownList per visualizzare i record 'master' e un controllo DataList per displ...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: 07fa47ae-e491-4a2f-b265-d342b9ddef46
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: c2199f0957f4cbe1d35dd971744087da9af1abce
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="masterdetail-filtering-with-a-dropdownlist-c"></a>Master-Details filtri con un controllo DropDownList (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare App di esempio](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_33_CS.exe) o [Scarica il PDF](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/datatutorial33cs1.pdf)

> In questa esercitazione viene illustrato come visualizzare i report master/dettaglio in una singola pagina web l'utilizzo di controlli DropDownList per visualizzare i record "master" e un controllo DataList per visualizzare i "Dettagli".


## <a name="introduction"></a>Introduzione

Il report master-details che abbiamo creati prima utilizzando un controllo GridView in precedenza [Master-Details filtro con un DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) esercitazione inizia con la visualizzazione di un set di record "master". L'utente può quindi drill-down in uno dei record master, in tal modo la visualizzazione "i dettagli." del record master Report master/dettaglio sono la scelta ideale per la visualizzazione delle relazioni uno-a-molti e per la visualizzazione di informazioni dettagliate dalle tabelle "wide" particolarmente (quelli che dispone di numerose colonne). Viene descritto come implementare i report master-Details mediante i controlli GridView e DetailsView nelle esercitazioni precedenti. In questa esercitazione e le due successive, è possibile riesaminare questi concetti, ma lo stato attivo sull'utilizzo di DataList e Repeater controlla invece.

In questa esercitazione verrà esaminato tramite un DropDownList per contenere il record "master" con "Dettagli" vengono visualizzati i record in un controllo DataList.

## <a name="step-1-adding-the-masterdetail-tutorial-web-pages"></a>Passaggio 1: Aggiunta di pagine Web esercitazione Master-Details

Prima di iniziare questa esercitazione, innanzitutto è opportuno aggiungere la cartella e le pagine ASP.NET che è necessario per questa esercitazione e le due successive gestiscono report master-Details mediante i controlli DataList e Repeater. Iniziare creando una nuova cartella nel progetto denominato `DataListRepeaterFiltering`. Successivamente, aggiungere le seguenti cinque pagine ASP.NET in questa cartella, con tutti gli elementi configurati per utilizzare la pagina master `Site.master`:

- `Default.aspx`
- `FilterByDropDownList.aspx`
- `CategoryListMaster.aspx`
- `ProductsForCategoryDetails.aspx`
- `CategoriesAndProducts.aspx`


![Creare una cartella DataListRepeaterFiltering e aggiungere le pagine ASP.NET dell'esercitazione](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image1.png)

**Figura 1**: creare un `DataListRepeaterFiltering` cartella e aggiungere le pagine ASP.NET dell'esercitazione


Aprire quindi il `Default.aspx` pagina e trascinare il `SectionLevelTutorialListing.ascx` controllo utente dal `UserControls` cartella nell'area di progettazione. Questo controllo utente, creata nel [pagine Master e esplorazione del sito](../introduction/master-pages-and-site-navigation-cs.md) esercitazione enumera la mappa del sito e visualizza le esercitazioni dalla sezione corrente in un elenco puntato.


[![Aggiungere il controllo utente SectionLevelTutorialListing.ascx Default.aspx](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image3.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image2.png)

**Figura 2**: aggiungere il `SectionLevelTutorialListing.ascx` controllo utente in `Default.aspx` ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image4.png))


Per visualizzare l'elenco puntato le esercitazioni master-details che verrà creata, è necessario aggiungerli alla mappa del sito. Aprire il `Web.sitemap` file e aggiungere il markup seguente dopo il tag di nodo "Visualizzazione di dati con il DataList e Repeater" mappa del sito:

[!code-xml[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample1.xml)]


![Aggiornare la mappa per includere le nuove pagine ASP.NET](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image5.png)

**Figura 3**: aggiornare la mappa per includere le nuove pagine ASP.NET


## <a name="step-2-displaying-the-categories-in-a-dropdownlist"></a>Passaggio 2: Visualizzare le categorie in un controllo DropDownList

Il report master-details elencherà le categorie in un controllo DropDownList, con i prodotti della voce di elenco selezionato visualizzati più in basso nella pagina un controllo DataList. La prima attività precedono us, è quindi, per le categorie visualizzate in un controllo DropDownList. Aprire il `FilterByDropDownList.aspx` nella pagina di `DataListRepeaterFiltering` cartelle e trascinare un controllo DropDownList dalla casella degli strumenti nella finestra di progettazione della pagina. Successivamente, impostare il DropDownList `ID` proprietà `Categories`. Fare clic sul collegamento smart tag del DropDownList Scegli origine dati e creare un nuovo oggetto ObjectDataSource denominato `CategoriesDataSource`.


[![Aggiungere un nuovo oggetto ObjectDataSource denominato CategoriesDataSource](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image7.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image6.png)

**Figura 4**: aggiungere un nuovo ObjectDataSource denominato `CategoriesDataSource` ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image8.png))


Configurare il nuovo oggetto ObjectDataSource in modo che richiama la `CategoriesBLL` della classe `GetCategories()` metodo. Dopo aver configurato il ObjectDataSource è comunque necessario specificare quale campo dell'origine dati deve essere visualizzato in DropDownList e che uno deve essere associato come valore per ogni elemento nell'elenco. Dispone il `CategoryName` campo come la visualizzazione e `CategoryID` come valore per ogni elemento nell'elenco.


[![Avere la visualizzazione di DropDownList campo CategoryName e CategoryID utilizzare come valore](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image10.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image9.png)

**Figura 5**: la visualizzazione DropDownList il `CategoryName` campo e utilizzare `CategoryID` come valore ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image11.png))


A questo punto si dispongono di un controllo DropDownList che viene popolato con i record di `Categories` tabella (all eseguita in circa sei secondi). Figura 6 mostra l'avanzamento finora quando viene visualizzato tramite un browser.


[![Un elenco a discesa sono elencate le categorie corrente](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image13.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image12.png)

**Figura 6**: elenco a discesa Elenca le categorie corrente ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image14.png))


## <a name="step-2-adding-the-products-datalist"></a>Passaggio 2: Aggiunta di prodotti DataList

L'ultimo passaggio nel report master-Details è per elencare i prodotti associati alla categoria selezionata. A tale scopo, aggiungere un controllo DataList alla pagina e creare un nuovo oggetto ObjectDataSource denominato `ProductsByCategoryDataSource`. Dispone il `ProductsByCategoryDataSource` controllo recuperare i dati di `ProductsBLL` della classe `GetProductsByCategoryID(categoryID)` (metodo). Poiché questo report master-Details è di sola lettura, scegliere il che opzione (nessuno) nelle schede delle INSERT, UPDATE e DELETE.


[![Selezionare il metodo GetProductsByCategoryID(categoryID)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image16.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image15.png)

**Figura 7**: selezionare il `GetProductsByCategoryID(categoryID)` metodo ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image17.png))


Dopo aver fatto clic su Avanti, la procedura guidata ObjectDataSource richiede us per l'origine del valore per il `GetProductsByCategoryID(categoryID)` del metodo  *`categoryID`*  parametro. Per utilizzare il valore dell'oggetto selezionato `categories` DropDownList elemento imposta l'origine di parametro al controllo e il ControlID per `Categories`.


[![Impostare il parametro categoryID al valore di DropDownList categorie](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image19.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image18.png)

**Figura 8**: impostare il  *`categoryID`*  il valore del parametro il `Categories` DropDownList ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image20.png))


Dopo avere completato la configurazione guidata origine dati, Visual Studio genera automaticamente un `ItemTemplate` per DataList che visualizza il nome e il valore di ogni campo dati. Consente di migliorare DataList per usare un `ItemTemplate` che visualizza solo il nome del prodotto, categoria, fornitore, quantità per unità e prezzo insieme a un `SeparatorTemplate` che inserisce un `<hr>` elemento tra ogni elemento. Ora utilizzare il `ItemTemplate` da in, ad esempio il [visualizzazione di dati con il DataList e Repeater](../displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-cs.md) esercitazione, ma è possibile utilizzare qualsiasi codice di modello trova più interessante.

Dopo aver apportato queste modifiche, del controllo DataList e markup ObjectDataSource dovrebbe essere simile al seguente:

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample2.aspx)]

Richiedere qualche istante per l'estrazione di stato di avanzamento in un browser. Durante la prima visita la pagina, vengono visualizzati i prodotti appartenenti alla categoria selezionata (bibite) (come illustrato nella figura 9), ma modifica DropDownList non aggiorna i dati. Infatti, deve verificarsi un postback per DataList da aggiornare. A tale scopo è possibile impostare il DropDownList `AutoPostBack` proprietà `true` o aggiungere un controllo pulsante Web alla pagina. Per questa esercitazione, è stato scelto per impostare il DropDownList `AutoPostBack` proprietà `true`.

Figure 9 e 10 viene illustrato il report master/dettaglio in azione.


[![Durante la prima visita la pagina, vengono visualizzati i prodotti delle bibite](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image22.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image21.png)

**Figura 9**: durante la prima visita la pagina, vengono visualizzati i prodotti delle bibite ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image23.png))


[![Selezionare automaticamente un nuovo prodotto (prodotto) provoca un PostBack, l'aggiornamento di DataList](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image25.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image24.png)

**Figura 10**: selezione di un nuovo prodotto (prodotto) automaticamente provoca un PostBack, l'aggiornamento di DataList ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image26.png))


## <a name="adding-a----choose-a-category----list-item"></a>Aggiunta di un elemento di elenco "-consente di scegliere una categoria"

Durante la visita prima la `FilterByDropDownList.aspx` pagina le categorie primo elemento di elenco del DropDownList (bibite) è selezionata per impostazione predefinita, che mostra i prodotti delle bibite in DataList. Nel *Master-Details filtro con un DropDownList* esercitazione è stata aggiunta un'opzione "-consente di scegliere una categoria," al DropDownList che è stato selezionato per impostazione predefinita e, se selezionata, vengono visualizzate *tutti* del prodotti nel database. Tale approccio è gestibile quando si elencano i prodotti in GridView, poiché ogni riga del prodotto richiedeva una piccola quantità di area dello schermo. Con DataList, tuttavia, le informazioni di ogni prodotto consuma una quantità maggiore di blocco schermo. Si è ancora aggiungere un'opzione "-consente di scegliere una categoria-" ed è selezionata per impostazione predefinita, ma anziché visualizzare tutti i prodotti quando selezionata, verrà configurato in modo da non Mostra prodotti.

Per aggiungere un nuovo elemento elenco DropDownList, passare alla finestra proprietà e fare clic sui puntini di sospensione nel `Items` proprietà. Aggiungere un nuovo elemento di elenco con il `Text` "-consente di scegliere una categoria-" e `Value` `0`.


![Aggiungere un](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image27.png)

**Figura 11**: aggiungere un elemento di elenco "-consente di scegliere una categoria"


In alternativa, è possibile aggiungere la voce di elenco aggiungendovi DropDownList il markup seguente:

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample3.aspx)]

Inoltre, è necessario impostare il controllo DropDownList `AppendDataBoundItems` a `true` perché se è impostato su `false` (impostazione predefinita), quando le categorie vengono associate a DropDownList da ObjectDataSource queste sovrascrivono qualsiasi elenco aggiunto manualmente elementi.


![Impostare la proprietà AppendDataBoundItems su True](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image28.png)

**Figura 12**: impostare il `AppendDataBoundItems` proprietà su True


Il motivo è stato scelto il valore `0` per l'elenco "-consente di scegliere una categoria," è perché vi sono categorie nel sistema con un valore di `0`, pertanto non verrà restituito alcun record di prodotto quando è selezionata la voce di elenco "-consente di scegliere una categoria,". A tale scopo, richiedere qualche istante per visitare la pagina tramite un browser. Come illustrato nella figura 13, quando inizialmente la visualizzazione della pagina è selezionata la voce di elenco "-consente di scegliere una categoria-" e nessun prodotto viene visualizzato.


[![Quando il](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image30.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image29.png)

**Figura 13**: quando è selezionata la voce di elenco "-consente di scegliere una categoria,", i prodotti non vengono visualizzati ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image31.png))


Se visualizza invece *tutti* dei prodotti quando è selezionata l'opzione "-consente di scegliere una categoria,", utilizzare un valore di `-1` invece. Il lettore attenti ricorderà che back nel *Master-Details filtro con un DropDownList* esercitazione è stato aggiornato il `ProductsBLL` della classe `GetProductsByCategoryID(categoryID)` metodo in modo che se un  *`categoryID`*  valore di `-1` passato, tutti i prodotti sono stati restituiti i record.

## <a name="summary"></a>Riepilogo

Quando si visualizzano dati correlati in modo gerarchico, è spesso utile per presentare i dati di utilizzo dei report master/dettaglio, da cui l'utente può iniziare ad analizzare i dati dalla parte superiore della gerarchia e drill-down nei dettagli. In questa esercitazione sono state esaminate la creazione di un report semplice master-details che mostri i prodotti della categoria selezionata. Questa operazione è stata eseguita utilizzando un controllo DropDownList per l'elenco di categorie e un controllo DataList per i prodotti appartenenti alla categoria selezionata.

Nella prossima esercitazione verrà esaminato separando i record master e dettagli su due pagine. Nella prima pagina, un elenco di record "master" verrà visualizzato con un collegamento per visualizzare i dettagli. Facendo clic sul collegamento verrà whisk all'utente la seconda pagina, che verrà visualizzati i dettagli per il record principale selezionato.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali...

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore per questa esercitazione è stata Randy Schmidt. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Successivo](master-detail-filtering-acess-two-pages-datalist-cs.md)
