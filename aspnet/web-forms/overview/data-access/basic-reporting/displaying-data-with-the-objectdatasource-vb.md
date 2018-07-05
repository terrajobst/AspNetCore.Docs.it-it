---
uid: web-forms/overview/data-access/basic-reporting/displaying-data-with-the-objectdatasource-vb
title: Visualizzazione dei dati con ObjectDataSource (VB) | Microsoft Docs
author: rick-anderson
description: Questa esercitazione esamina il controllo ObjectDataSource tramite questo controllo che è possibile associare i dati recuperati da BLL creata nell'esercitazione precedente senza connettività...
ms.author: aspnetcontent
ms.date: 03/31/2010
ms.assetid: d62c3a63-0940-4019-874e-4a4047df0c1c
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/displaying-data-with-the-objectdatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: 759403bf175cfb2d44cc53ab497c55c354733f15
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832724"
---
<a name="displaying-data-with-the-objectdatasource-vb"></a>Visualizzazione dei dati con ObjectDataSource (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'App di esempio](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_4_VB.exe) o [Scarica il PDF](displaying-data-with-the-objectdatasource-vb/_static/datatutorial04vb1.pdf)

> Questa esercitazione esamina il controllo ObjectDataSource tramite questo controllo che è possibile associare i dati recuperati da BLL creata nell'esercitazione precedente senza dover scrivere una riga di codice!


## <a name="introduction"></a>Introduzione

Con l'applicazione architettura e il sito Web layout di pagina completo, siamo pronti per iniziare a esplorare come eseguire un'ampia gamma di attività comuni e reporting-relative ai dati. Nelle esercitazioni precedenti abbiamo visto come associare programmaticamente i dati DAL e BLL a un controllo Web in una pagina ASP.NET di dati. Questa sintassi dei dati di controllo Web di assegnazione `DataSource` proprietà ai dati da visualizzare e chiamando quindi il controllo `DataBind()` metodo è stato il motivo utilizzato nelle applicazioni ASP.NET 1.x e può continuare a usare nelle 2.0 applicazioni. Tuttavia, nuovi controlli di origine dati ASP.NET 2.0 offrono una modalità dichiarativa per lavorare con i dati. Uso di questi controlli è possibile associare i dati recuperati da BLL creata nell'esercitazione precedente senza dover scrivere una riga di codice!

ASP.NET 2.0 viene fornito con cinque controlli origine dati incorporata [SqlDataSource](https://msdn.microsoft.com/library/dz12d98w%28vs.80%29.aspx), [AccessDataSource](https://msdn.microsoft.com/library/8e5545e1.aspx), [ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx), [XmlDataSource](https://msdn.microsoft.com/library/e8d8587a%28en-US,VS.80%29.aspx), e [SiteMapDataSource](https://msdn.microsoft.com/library/5ex9t96x%28en-US,VS.80%29.aspx) anche se è possibile compilare il proprio [controlli origine dati personalizzati](https://msdn.microsoft.com/library/default.asp?url=/library/dnvs05/html/DataSourceCon1.asp), se necessario. Poiché abbiamo sviluppato un'architettura di applicazione della nostra esercitazione, utilizzeremo ObjectDataSource contro le nostre classi BLL.


![ASP.NET 2.0 include cinque controlli origine dati incorporata](displaying-data-with-the-objectdatasource-vb/_static/image1.png)

**Figura 1**: ASP.NET 2.0 include cinque controlli origine dati incorporata


ObjectDataSource funge da proxy per l'uso di un altro oggetto. Per configurare ObjectDataSource, specifichiamo l'oggetto e delle corrispondenze tra i relativi metodi di ObjectDataSource `Select`, `Insert`, `Update`, e `Delete` metodi. Dopo che è stato specificato l'oggetto sottostante e mappato i suoi metodi di ObjectDataSource, è quindi possibile associare ObjectDataSource a un controllo Web per dati. ASP.NET viene fornito con dati di molti controlli Web, tra cui il controllo GridView, DetailsView, RadioButtonList e DropDownList, tra gli altri. Durante il ciclo di vita di pagina, i controllo Web per dati potrebbe essere necessario accedere ai dati è associato a, che completerà richiamando ObjectDataSource `Select` metodo; se i controllo Web per dati supportano l'inserimento, aggiornamento, o l'eliminazione, chiamate possono essere reso a relativo ObjectDataSource `Insert`, `Update`, o `Delete` metodi. Queste chiamate vengono poi indirizzate da ObjectDataSource ai metodi dell'oggetto sottostante appropriato, come illustrato nel diagramma seguente.


[![ObjectDataSource funge da Proxy](displaying-data-with-the-objectdatasource-vb/_static/image3.png)](displaying-data-with-the-objectdatasource-vb/_static/image2.png)

**Figura 2**: l'oggetto ObjectDataSource funge da Proxy ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-data-with-the-objectdatasource-vb/_static/image4.png))


Mentre ObjectDataSource può essere utilizzato per richiamare i metodi per l'inserimento, aggiornamento o eliminazione di dati, è possibile concentrarsi solo sui dati restituiti; nelle esercitazioni successive verranno illustrato l'utilizzo di ObjectDataSource e dei controlli Web che modificano i dati.

## <a name="step-1-adding-and-configuring-the-objectdatasource-control"></a>Passaggio 1: Aggiunta e configurazione del controllo ObjectDataSource

Iniziare aprendo il `SimpleDisplay.aspx` nella pagina di `BasicReporting` cartella, passare alla visualizzazione progettazione e quindi trascinare un controllo ObjectDataSource dalla casella degli strumenti nell'area di progettazione della pagina. ObjectDataSource ha l'aspetto di un riquadro grigio nell'area di progettazione perché non produce alcun markup; accede semplicemente ai dati richiamando un metodo da un oggetto specificato. I dati restituiti da ObjectDataSource possono essere visualizzati da un controllo Web, ad esempio il controllo GridView, DetailsView, FormView e così via di dati.

> [!NOTE]
> In alternativa, è prima può aggiungere i dati di controllo Web alla pagina e quindi dal suo smart tag, scegliere il &lt;nuova origine dati&gt; opzione nell'elenco a discesa.


Per specificare l'oggetto sottostante di ObjectDataSource e i metodi dell'oggetto mapping di ObjectDataSource, fare clic sul collegamento Configura origine dati dallo smart tag di ObjectDataSource.


[![Scegliere il collegamento Configura origine dati nello Smart tag](displaying-data-with-the-objectdatasource-vb/_static/image6.png)](displaying-data-with-the-objectdatasource-vb/_static/image5.png)

**Figura 3**: fare clic sul collegamento Configura di un origine dei dati nello Smart tag ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-data-with-the-objectdatasource-vb/_static/image7.png))


Verrà visualizzata la procedura guidata Configura origine dati. In primo luogo, dobbiamo specificare l'oggetto che ObjectDataSource deve lavorare. Se è selezionata la casella di controllo "Mostra solo componenti dati", l'elenco a discesa in questa schermata vengono elencati solo gli oggetti che sono stato contrassegnati con il `DataObject` attributo. Attualmente il nostro elenco include i TableAdapter del DataSet tipizzato e le classi BLL creata nell'esercitazione precedente. Se si è dimenticato di aggiungere il `DataObject` attributo alle classi di livello per la logica di Business non verranno riportati in questo elenco. In tal caso, deselezionare la casella di controllo "Mostra solo componenti dati" per visualizzare tutti gli oggetti, che devono includere le classi BLL (assieme alle altre classi nel DataSet tipizzato oggetti DataTable, DataRow e così via).

Nella prima schermata scegliere il `ProductsBLL` classe dall'elenco a discesa elenco e fare clic su Avanti.


[![Specificare l'oggetto da utilizzare con il controllo ObjectDataSource](displaying-data-with-the-objectdatasource-vb/_static/image9.png)](displaying-data-with-the-objectdatasource-vb/_static/image8.png)

**Figura 4**: specificare l'oggetto da utilizzare con il controllo ObjectDataSource ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-data-with-the-objectdatasource-vb/_static/image10.png))


Nella schermata successiva della procedura guidata chiederà di selezionare il metodo che ObjectDataSource deve richiamare. Elenco a discesa sono elencati i metodi che restituiscono dati per l'oggetto selezionato nella schermata precedente. Qui vediamo `GetProductByProductID`, `GetProducts`, `GetProductsByCategoryID`, e `GetProductsBySupplierID`. Selezionare il `GetProducts` metodo dall'elenco a discesa e fare clic su Fine (se è stato aggiunto il `DataObjectMethodAttribute` per il `ProductBLL`di metodi come illustrato nell'esercitazione precedente, questa opzione saranno selezionati per impostazione predefinita).


[![Scegliere il metodo per la restituzione di dati nella scheda Seleziona](displaying-data-with-the-objectdatasource-vb/_static/image12.png)](displaying-data-with-the-objectdatasource-vb/_static/image11.png)

**Figura 5**: scegliere il metodo per restituire i dati dalla scheda selezionare ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-data-with-the-objectdatasource-vb/_static/image13.png))


## <a name="configure-the-objectdatasource-manually"></a>Configurare manualmente ObjectDataSource

Procedura guidata Configura origine dati di ObjectDataSource costituisce un metodo rapido per specificare l'oggetto da utilizzare e per associare ai metodi dell'oggetto vengono richiamati. Tuttavia, è possibile configurare ObjectDataSource mediante le relative proprietà, nella finestra delle proprietà o direttamente nel markup dichiarativo. È sufficiente impostare il `TypeName` proprietà per il tipo dell'oggetto sottostante da utilizzare e il `SelectMethod` al metodo da richiamare quando il recupero dei dati.


[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample1.aspx)]

Anche se si preferisce la procedura guidata Configura origine dati potrebbero esserci casi è necessario configurare manualmente ObjectDataSource, come la procedura guidata elenca solo le classi create dallo sviluppatore. Se si desidera associare ObjectDataSource a una classe in .NET Framework, ad esempio la [classe di appartenenza](https://msdn.microsoft.com/library/system.web.security.membership.aspx), per accedere alle informazioni dell'account utente, o il [classe Directory](https://msdn.microsoft.com/library/system.io.directory.aspx) per lavorare con informazioni sul file system è necessario impostare manualmente le proprietà di ObjectDataSource.

## <a name="step-2-adding-a-data-web-control-and-binding-it-to-the-objectdatasource"></a>Passaggio 2: Aggiunta di un controllo Web per dati e associarlo a ObjectDataSource

Una volta ObjectDataSource è stato aggiunto alla pagina e configurato, siamo pronti per aggiungere controlli Web dei dati alla pagina per visualizzare i dati restituiti da ObjectDataSource `Select` (metodo). Qualsiasi controllo Web per dati possono essere associati a ObjectDataSource; Vediamo ora come visualizzare i dati di ObjectDataSource in un controllo GridView, DetailsView e FormView.

## <a name="binding-a-gridview-to-the-objectdatasource"></a>Associazione di un controllo GridView a ObjectDataSource

Aggiungere un controllo GridView dalla casella degli strumenti `SimpleDisplay.aspx`dell'area di progettazione. Dallo smart tag del controllo GridView, scegliere il controllo ObjectDataSource che aggiunto nel passaggio 1. Verrà creato automaticamente un BoundField in GridView per ciascuna proprietà restituita in base ai dati da ObjectDataSource `Select` metodo (vale a dire, le proprietà definite nel DataTable dei prodotti).


[![Un controllo GridView è stato aggiunto alla pagina e associato a ObjectDataSource](displaying-data-with-the-objectdatasource-vb/_static/image15.png)](displaying-data-with-the-objectdatasource-vb/_static/image14.png)

**Figura 6**: un controllo GridView è stato aggiunto alla pagina e associato a ObjectDataSource ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-data-with-the-objectdatasource-vb/_static/image16.png))


È quindi possibile personalizzare, ridisporre o rimuovere i BoundField di GridView facendo clic sull'opzione Modifica colonne dello smart tag.


[![Gestire i BoundField di GridView mediante la finestra di dialogo Modifica colonne](displaying-data-with-the-objectdatasource-vb/_static/image18.png)](displaying-data-with-the-objectdatasource-vb/_static/image17.png)

**Figura 7**: la gestione BoundField tramite la modifica finestra del controllo GridView di dialogo colonne ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-data-with-the-objectdatasource-vb/_static/image19.png))


Si consiglia di modificare i BoundField di GridView, rimuovendo il `ProductID`, `SupplierID`, `CategoryID`, `QuantityPerUnit`, `UnitsInStock`, `UnitsOnOrder`, e `ReorderLevel` BoundField. È sufficiente selezionare i BoundField dall'elenco in basso a sinistra e fare clic sul pulsante di eliminazione (la X rossa) per rimuoverli. Ridisporre i BoundField in modo che il `CategoryName` e `SupplierName` precedano il `UnitPrice` BoundField selezionando questi BoundField e facendo clic sulla freccia. Impostare il `HeaderText` delle proprietà dei restanti BoundField per `Products`, `Category`, `Supplier`, e `Price`, rispettivamente. Chiedere quindi il `Price` BoundField price come valuta impostando i BoundField `HtmlEncode` la proprietà su False e la relativa `DataFormatString` proprietà `{0:c}`. Infine, allineare orizzontalmente il `Price` a destra e la `Discontinued` casella di controllo al centro mediante il `ItemStyle` / `HorizontalAlign` proprietà.


[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample2.aspx)]


[![Sono stati personalizzati i BoundField di GridView](displaying-data-with-the-objectdatasource-vb/_static/image21.png)](displaying-data-with-the-objectdatasource-vb/_static/image20.png)

**Figura 8**: sono stati personalizzati di GridView BoundField ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-data-with-the-objectdatasource-vb/_static/image22.png))


## <a name="using-themes-for-a-consistent-look"></a>Utilizzo dei temi per un aspetto coerente

Queste esercitazioni cercheremo di rimuovere tutte le impostazioni di stile a livello di controllo, usando invece i fogli di stile definiti in un file esterno ogni volta che è possibile. Il `Styles.css` file contiene `DataWebControlStyle`, `HeaderStyle`, `RowStyle`, e `AlternatingRowStyle` classi CSS che devono essere utilizzate per determinare l'aspetto dei dati di controlli Web utilizzati in queste esercitazioni. A tale scopo, possiamo impostare il controllo GridView `CssClass` proprietà `DataWebControlStyle`e i relativi `HeaderStyle`, `RowStyle`, e `AlternatingRowStyle` proprietà `CssClass` le proprietà di conseguenza.

Se impostassimo queste `CssClass` proprietà a livello del controllo Web dovremmo ricordarci di impostare esplicitamente i valori delle proprietà per i dati di ogni controllo a esercitazioni Web. Un approccio più gestibile consiste nel definire le proprietà CSS predefinite per GridView, DetailsView e FormView utilizzando un tema. Un tema è una raccolta di impostazioni delle proprietà a livello di controllo, immagini e classi CSS che possono essere applicate alle pagine di un sito per garantire un aspetto comune e coerente.

Il nostro tema non includerà Nessuna immagine o file CSS (lasceremo il foglio di stile `Styles.css` come-è, definito nella cartella radice dell'applicazione web), ma includerà due interfacce. Un'interfaccia è un file che definisce le proprietà predefinite per un controllo Web. In particolare, avremo un file di interfaccia per i controlli GridView e DetailsView, che indica il valore predefinito `CssClass`-proprietà correlate.

Iniziare aggiungendo un nuovo File di interfaccia al progetto denominato `GridView.skin` facendo clic sul nome del progetto in Esplora soluzioni e scegliendo Aggiungi nuovo elemento.


[![Aggiungere un File di interfaccia denominato GridView. skin](displaying-data-with-the-objectdatasource-vb/_static/image24.png)](displaying-data-with-the-objectdatasource-vb/_static/image23.png)

**Figura 9**: aggiungere un File di interfaccia denominato `GridView.skin` ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-data-with-the-objectdatasource-vb/_static/image25.png))


I file di interfaccia devono essere inseriti in un tema che si trovano nel `App_Themes` cartella. Poiché non abbiamo ancora tale cartella, Visual Studio chiederà di crearne uno automaticamente quando si aggiunge la prima interfaccia. Fare clic su Sì per creare il `App_Theme` cartella e inserirvi il nuovo `GridView.skin` file non esiste.


[![Consenti a Visual Studio crea la cartella App_Theme](displaying-data-with-the-objectdatasource-vb/_static/image27.png)](displaying-data-with-the-objectdatasource-vb/_static/image26.png)

**Figura 10**: consentire a Visual Studio crea il `App_Theme` cartella ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-data-with-the-objectdatasource-vb/_static/image28.png))


Verrà creato un nuovo tema nella `App_Themes` cartella denominato GridView con il file di interfaccia `GridView.skin`.


![Il tema GridView è stato aggiunto alla cartella App_Theme](displaying-data-with-the-objectdatasource-vb/_static/image29.png)

**Figura 11**: il tema GridView è stato aggiunto il `App_Theme` cartella


Rinominare il tema GridView in DataWebControls (pulsante destro del mouse sulla cartella GridView nella `App_Theme` cartella e scegliere Rinomina). Successivamente, immettere il seguente markup nel `GridView.skin` file:


[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample3.aspx)]

Definisce le proprietà predefinite per il `CssClass`-proprietà correlate per tutti i GridView in qualsiasi pagina che usa il tema DataWebControls. È possibile aggiungere un'altra interfaccia per DetailsView, un controllo Web che verrà usato subito i dati. Aggiungere un'interfaccia al tema DataWebControls denominata `DetailsView.skin` e aggiungere il markup seguente:


[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample4.aspx)]

Con il nostro tema definito, l'ultimo passaggio consiste nell'applicare il tema alla nostra pagina ASP.NET. Un tema può essere applicato in base a una pagina per pagina o per tutte le pagine in un sito Web. È possibile utilizzare questo tema per tutte le pagine nel sito Web. A tale scopo, aggiungere il markup seguente alla `Web.config`del `<system.web>` sezione:


[!code-xml[Main](displaying-data-with-the-objectdatasource-vb/samples/sample5.xml)]

Questo è tutto qui! Il `styleSheetTheme` impostazione indica che le proprietà specificate nel tema dovrebbero *non* le proprietà specificate a livello di controllo. Per specificare che le impostazioni del tema non devono escludere le impostazioni di controllo, usare il `theme` dell'attributo al posto di `styleSheetTheme`; Sfortunatamente, le impostazioni del tema non vengono visualizzati nella visualizzazione progettazione di Visual Studio. Fare riferimento a [Cenni preliminari su interfacce e temi ASP.NET](https://msdn.microsoft.com/library/ykzx33wh.aspx) e [Server-Side Styles Using Themes](https://quickstarts.asp.net/quickstartv20/aspnet/doc/themes/stylesheettheme.aspx) per altre informazioni su temi e interfacce; vedere [How To: applicare temi ASP.NET](https://msdn.microsoft.com/library/0yy5hxdk%28VS.80%29.aspx) per altre informazioni su configurazione di una pagina per l'uso di un tema.


[![GridView Visualizza nome del prodotto, categoria, fornitore, prezzo e le informazioni obsolete](displaying-data-with-the-objectdatasource-vb/_static/image31.png)](displaying-data-with-the-objectdatasource-vb/_static/image30.png)

**Figura 12**: GridView Visualizza nome del prodotto, categoria, fornitore, prezzo e informazioni non più disponibili ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-data-with-the-objectdatasource-vb/_static/image32.png))


## <a name="displaying-one-record-at-a-time-in-the-detailsview"></a>Visualizzazione di un Record alla volta in DetailsView

GridView Visualizza una riga per ogni record restituito dal controllo origine dati a cui è associato. Esistono casi, tuttavia, quando potremmo voler visualizzare un record o un solo record alla volta. Il [controllo DetailsView](https://msdn.microsoft.com/library/s3w1w7t4.aspx) offre questa funzionalità, il rendering come HTML `<table>` con due colonne e una riga per ogni colonna o una proprietà associata al controllo. Può pensare a DetailsView come a un GridView con un singolo record ruotato di 90 gradi.

Iniziare aggiungendo un controllo DetailsView *sopra* GridView in `SimpleDisplay.aspx`. Successivamente, eseguirne l'associazione allo stesso controllo ObjectDataSource di GridView. Come in GridView, verrà aggiunto un BoundField di DetailsView per ogni proprietà nell'oggetto restituito da ObjectDataSource `Select` (metodo). L'unica differenza è che i BoundField di DetailsView sono disposti orizzontalmente anziché verticalmente.


[![Aggiungere un controllo DetailsView alla pagina e associarlo a ObjectDataSource](displaying-data-with-the-objectdatasource-vb/_static/image34.png)](displaying-data-with-the-objectdatasource-vb/_static/image33.png)

**Figura 13**: aggiungere un controllo DetailsView alla pagina e associarlo a ObjectDataSource ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-data-with-the-objectdatasource-vb/_static/image35.png))


Come per GridView, i BoundField di DetailsView possono essere modificati per fornire una visualizzazione più personalizzata dei dati restituiti da ObjectDataSource. Figura 14 mostra DetailsView dopo i BoundField e `CssClass` proprietà sono state configurate per rendere l'aspetto simile all'esempio GridView.


[![DetailsView Mostra un singolo Record](displaying-data-with-the-objectdatasource-vb/_static/image37.png)](displaying-data-with-the-objectdatasource-vb/_static/image36.png)

**Figura 14**: DetailsView Mostra un singolo Record ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-data-with-the-objectdatasource-vb/_static/image38.png))


Si noti che DetailsView visualizza solo il primo record restituito dall'origine dati. Per consentire all'utente di scorrere i record uno alla volta, è necessario attivare il paging in DetailsView. A tale scopo, tornare a Visual Studio e selezionare la casella di controllo Attiva Paging nello smart tag di DetailsView.


[![Attivare il Paging nel controllo DetailsView](displaying-data-with-the-objectdatasource-vb/_static/image40.png)](displaying-data-with-the-objectdatasource-vb/_static/image39.png)

**Figura 15**: Abilita Paging nel controllo DetailsView ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-data-with-the-objectdatasource-vb/_static/image41.png))


[![Con il Paging attivato, DetailsView consente all'utente di visualizzare tutti i prodotti](displaying-data-with-the-objectdatasource-vb/_static/image43.png)](displaying-data-with-the-objectdatasource-vb/_static/image42.png)

**Figura 16**: con Paging attivato, DetailsView consente all'utente di visualizzare tutti i prodotti ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-data-with-the-objectdatasource-vb/_static/image44.png))


Ci soffermeremo maggiormente sulla funzione di paging in futuro le esercitazioni.

## <a name="a-more-flexible-layout-for-showing-one-record-at-a-time"></a>Un Layout più flessibile per visualizzare un Record alla volta

Il controllo DetailsView è alquanto rigido nel modo in cui visualizza ciascun record restituito da ObjectDataSource. Talvolta potremmo desiderare una visualizzazione più flessibile dei dati. Ad esempio, invece di visualizzare il nome del prodotto, categoria, fornitore, prezzo e non più disponibili informazioni su una riga distinta, si potrebbe essere necessario visualizzare il nome del prodotto e il prezzo in un `<h4>` intestazione, con le informazioni di categoria e il fornitore visualizzati sotto il nome e il prezzo in una dimensione inferiore. E abbiamo potremmo non voler visualizzare i nomi delle proprietà (prodotto, categoria e così via) accanto ai valori di.

Il [controllo FormView](https://msdn.microsoft.com/library/fyf1dk77.aspx) offre questo livello di personalizzazione. Piuttosto che utilizzare campi (come fanno GridView e DetailsView), FormView utilizza modelli che consentono di avvalersi dei controlli Web, HTML statico, e [sintassi di associazione dati](http://www.15seconds.com/issue/040630.htm). Se si ha familiarità con il controllo Repeater da ASP.NET 1.x, può pensare a FormView come Repeater che consente di visualizzare un solo record.

Aggiungere un controllo FormView la `SimpleDisplay.aspx` nell'area di progettazione della pagina. Inizialmente FormView viene visualizzato come un blocco grigio, informarci che dobbiamo fornire, il requisito minimo del controllo `ItemTemplate`.


[![Il controllo FormView deve includere un ItemTemplate](displaying-data-with-the-objectdatasource-vb/_static/image46.png)](displaying-data-with-the-objectdatasource-vb/_static/image45.png)

**Figura 17**: The FormView deve contenere un `ItemTemplate` ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-data-with-the-objectdatasource-vb/_static/image47.png))


È possibile associare FormView direttamente a un controllo origine dati tramite la smart tag di FormView, che creerà un valore predefinito `ItemTemplate` automaticamente (insieme a un `EditItemTemplate` e `InsertItemTemplate`, se il controllo ObjectDatatSource `InsertMethod` e`UpdateMethod` vengono impostate proprietà). Tuttavia, per questo esempio è possibile associare i dati a FormView e specificare il `ItemTemplate` manualmente. Per iniziare, l'impostazione del controllo FormView `DataSourceID` proprietà per il `ID` del controllo ObjectDataSource, `ObjectDataSource1`. A questo punto, creare il `ItemTemplate` in modo da visualizzare nome del prodotto e prezzo in un `<h4>` elemento e i nomi di categoria e del fornitore sotto questo in una dimensione inferiore.


[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample6.aspx)]


[![Il primo prodotto (Chai) viene visualizzato in un formato personalizzato](displaying-data-with-the-objectdatasource-vb/_static/image49.png)](displaying-data-with-the-objectdatasource-vb/_static/image48.png)

**Figura 18**: il primo prodotto (Chai) viene visualizzato in un formato personalizzato ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-data-with-the-objectdatasource-vb/_static/image50.png))


Il `<%# Eval(propertyName) %>` è riportata la sintassi di associazione dati. Il `Eval` metodo restituisce il valore della proprietà specificata per l'oggetto corrente da associare al controllo FormView. Consultare l'articolo di Alex Homer [semplificato ed Extended Data Binding Syntax in ASP.NET 2.0](http://www.15seconds.com/issue/040630.htm) per altre informazioni su vantaggi e svantaggi dell'associazione dati.

Analogamente a DetailsView, FormView mostra solo il primo record restituito da ObjectDataSource. È possibile abilitare il paging in FormView per consentire agli utenti di scorrere i prodotti uno alla volta.

## <a name="summary"></a>Riepilogo

L'accesso e visualizzazione dei dati da un livello di logica di Business può essere eseguite senza scrivere una riga di codice grazie al controllo ObjectDataSource di ASP.NET 2.0. ObjectDataSource richiama un metodo specifico di una classe e restituisce i risultati. Questi risultati possono essere visualizzati in un controllo Web associato a ObjectDataSource di dati. In questa esercitazione abbiamo imparato ad associare i controlli GridView, DetailsView e FormView a ObjectDataSource.

Finora abbiamo visto solo come utilizzare ObjectDataSource per richiamare un metodo senza parametri, ma cosa accade se si vuole richiamare un metodo che prevede parametri di input, ad esempio la `ProductBLL` della classe `GetProductsByCategoryID(categoryID)`? Per chiamare un metodo che prevede uno o più parametri, dobbiamo configurare ObjectDataSource per specificare i valori per questi parametri. Si vedrà come eseguire questa operazione nel nostro [prossima esercitazione](declarative-parameters-vb.md).

Buona programmazione!

## <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Creare i propri controlli origine dati](https://msdn.microsoft.com/library/ms364049.aspx)
- [GridView Examples for ASP.NET 2.0](https://msdn.microsoft.com/library/aa479339.aspx)
- [Simplified and Extended Data Binding Syntax in ASP.NET 2.0](http://www.15seconds.com/issue/040630.htm)
- [Temi in ASP.NET 2.0](http://www.odetocode.com/Articles/423.aspx)
- [Server-Side Styles Using Themes](https://quickstarts.asp.net/quickstartv20/aspnet/doc/themes/stylesheettheme.aspx)
- [Procedura: Applicare temi ASP.NET programmaticamente](https://msdn.microsoft.com/library/tx35bd89.aspx)

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Hilton Giesenow, revisore per questa esercitazione. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](programmatically-setting-the-objectdatasource-s-parameter-values-cs.md)
> [Successivo](declarative-parameters-vb.md)
