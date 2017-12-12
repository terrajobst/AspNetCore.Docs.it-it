---
uid: web-forms/overview/data-access/basic-reporting/displaying-data-with-the-objectdatasource-vb
title: Visualizzazione dei dati con ObjectDataSource (VB) | Documenti Microsoft
author: rick-anderson
description: "In questa esercitazione vengono esaminati controllo ObjectDataSource utilizzando questo controllo che è possibile associare i dati recuperati da BLL creato nell'esercitazione precedente senza connettività..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: d62c3a63-0940-4019-874e-4a4047df0c1c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/displaying-data-with-the-objectdatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: d575c8f597bcb5d2a5d2e27e1145d39110daabe1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="displaying-data-with-the-objectdatasource-vb"></a>Visualizzazione dei dati con ObjectDataSource (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare App di esempio](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_4_VB.exe) o [Scarica il PDF](displaying-data-with-the-objectdatasource-vb/_static/datatutorial04vb1.pdf)

> In questa esercitazione vengono esaminati controllo ObjectDataSource utilizzando questo controllo che è possibile associare i dati recuperati da BLL creato nell'esercitazione precedente senza dover scrivere una riga di codice.


## <a name="introduction"></a>Introduzione

Con l'applicazione architettura e il sito Web layout di pagina completamento, saremo pronti iniziare a esplorare come eseguire un'ampia gamma di attività e report-correlate ai dati comuni. Nelle esercitazioni precedenti abbiamo visto come associare a livello di programmazione dati di DAL e BLL a un controllo Web in una pagina ASP.NET di dati. Questa sintassi di assegnazione dei dati di controllo Web `DataSource` proprietà ai dati da visualizzare e chiamando quindi il controllo `DataBind()` metodo è stato al modello utilizzato per le applicazioni ASP.NET 1. x e possono continuare a essere utilizzato nelle 2.0 applicazioni. Tuttavia, nuovi controlli di origine dati ASP.NET 2.0 offrono una modalità dichiarativa per funzionare con dati. Utilizzo di questi controlli è possibile associare dati recuperati da BLL creato nell'esercitazione precedente senza dover scrivere una riga di codice.

ASP.NET 2.0 viene fornito con cinque controlli origine dati incorporata [SqlDataSource](https://msdn.microsoft.com/en-us/library/dz12d98w%28vs.80%29.aspx), [AccessDataSource](https://msdn.microsoft.com/en-us/library/8e5545e1.aspx), [ObjectDataSource](https://msdn.microsoft.com/en-us/library/9a4kyhcx.aspx), [XmlDataSource](https://msdn.microsoft.com/en-us/library/e8d8587a%28en-US,VS.80%29.aspx), e [SiteMapDataSource](https://msdn.microsoft.com/en-us/library/5ex9t96x%28en-US,VS.80%29.aspx) anche se è possibile compilare [controlli origine dati personalizzati](https://msdn.microsoft.com/library/default.asp?url=/library/en-us/dnvs05/html/DataSourceCon1.asp), se necessario. Poiché è stato sviluppato un'architettura per l'applicazione di esercitazione, verrà usato ObjectDataSource contro le nostre classi BLL.


![ASP.NET 2.0 include cinque controlli origine dati incorporata](displaying-data-with-the-objectdatasource-vb/_static/image1.png)

**Figura 1**: ASP.NET 2.0 include cinque controlli origine dati incorporata


ObjectDataSource funge da proxy per l'utilizzo di un altro oggetto. Per configurare ObjectDataSource è specificare sottostante di oggetto e il mappano tra i metodi di ObjectDataSource `Select`, `Insert`, `Update`, e `Delete` metodi. Dopo che è stato specificato l'oggetto sottostante e i metodi di cui è stato eseguito il mapping di ObjectDataSource, è possibile associare ObjectDataSource a un controllo Web di dati. ASP.NET viene fornito con i dati di molti controlli Web, ad esempio, il GridView, DetailsView, RadioButtonList e DropDownList, tra gli altri. Durante il ciclo di vita di pagina, i dati di controllo Web potrebbero essere necessario accedere ai dati di cui è associato a, completeranno richiamando ObjectDataSource `Select` metodo; se i dati di controllo Web supportano l'inserimento, aggiornamento, eliminazione, è possibile richiamare per il ObjectDataSource `Insert`, `Update`, o `Delete` metodi. Queste chiamate vengono quindi indirizzate da ObjectDataSource ai metodi dell'oggetto sottostante appropriata come illustrato nel diagramma seguente.


[![ObjectDataSource funge da Proxy](displaying-data-with-the-objectdatasource-vb/_static/image3.png)](displaying-data-with-the-objectdatasource-vb/_static/image2.png)

**Figura 2**: il ObjectDataSource funge da Proxy ([fare clic per visualizzare l'immagine ingrandita](displaying-data-with-the-objectdatasource-vb/_static/image4.png))


Mentre ObjectDataSource può essere utilizzato per richiamare i metodi per l'inserimento, aggiornamento o eliminazione di dati, solo concentriamoci sulla restituzione di dati. nelle esercitazioni successive verranno illustrato l'utilizzo di ObjectDataSource e dei controlli Web che modificano i dati.

## <a name="step-1-adding-and-configuring-the-objectdatasource-control"></a>Passaggio 1: Aggiunta e configurazione del controllo ObjectDataSource

Aprire il `SimpleDisplay.aspx` nella pagina di `BasicReporting` cartella, passare alla visualizzazione progettazione e quindi trascinare un controllo ObjectDataSource dalla casella degli strumenti nell'area di progettazione della pagina. ObjectDataSource viene visualizzato come casella grigia nell'area di progettazione perché non produce alcun markup. è sufficiente, accede ai dati richiamando un metodo da un oggetto specificato. I dati restituiti da ObjectDataSource possono essere visualizzati da un controllo Web, ad esempio GridView, DetailsView, FormView e così via di dati.

> [!NOTE]
> In alternativa, prima di aggiungere i dati di controllo Web alla pagina e dal suo smart tag, quindi scegliere il &lt;nuova origine dati&gt; opzione dall'elenco a discesa.


Per specificare l'oggetto sottostante di ObjectDataSource e il mappano tra i metodi dell'oggetto di ObjectDataSource, fare clic sul collegamento Configura origine dati smart tag di ObjectDataSource.


[![Scegliere il collegamento Configura origine dati dallo Smart Tag](displaying-data-with-the-objectdatasource-vb/_static/image6.png)](displaying-data-with-the-objectdatasource-vb/_static/image5.png)

**Figura 3**: fare clic sul collegamento origine dati configurare Smart tag ([fare clic per visualizzare l'immagine ingrandita](displaying-data-with-the-objectdatasource-vb/_static/image7.png))


Verrà visualizzata la configurazione guidata origine dati. In primo luogo, è necessario specificare l'oggetto che ObjectDataSource si intende utilizzare. Se è selezionata la casella di controllo "Mostra solo componenti dati", l'elenco a discesa in questa schermata vengono elencati solo gli oggetti che hanno ricevuto il `DataObject` attributo. Attualmente questo elenco sono inclusi gli oggetti TableAdapter del DataSet tipizzato e le classi BLL creata nell'esercitazione precedente. Se si è dimenticato di aggiungere il `DataObject` attributo alle classi di livello di logica di Business vengono non verrà visualizzate in questo elenco. In tal caso, deselezionare la casella di controllo "Mostra solo componenti dati" per visualizzare tutti gli oggetti, che devono includere le classi BLL (insieme ad altre classi il DataSet tipizzato DataTable DataRow e così via).

Nella prima schermata scegliere il `ProductsBLL` classe dall'elenco a discesa e fare clic su Avanti.


[![Specificare l'oggetto da utilizzare con il controllo ObjectDataSource](displaying-data-with-the-objectdatasource-vb/_static/image9.png)](displaying-data-with-the-objectdatasource-vb/_static/image8.png)

**Figura 4**: specificare l'oggetto da utilizzare con il controllo ObjectDataSource ([fare clic per visualizzare l'immagine ingrandita](displaying-data-with-the-objectdatasource-vb/_static/image10.png))


La schermata successiva della procedura guidata viene richiesto di selezionare il metodo ObjectDataSource deve richiamare. Elenco a discesa sono elencati i metodi che restituiscono dati per l'oggetto selezionato nella schermata precedente. Ecco `GetProductByProductID`, `GetProducts`, `GetProductsByCategoryID`, e `GetProductsBySupplierID`. Selezionare il `GetProducts` metodo dall'elenco a discesa e fare clic su Fine (se è stato aggiunto il `DataObjectMethodAttribute` per il `ProductBLL`di metodi, come illustrato nell'esercitazione precedente, questa opzione verranno selezionati per impostazione predefinita).


[![Scegliere il metodo per restituire i dati dalla scheda Seleziona](displaying-data-with-the-objectdatasource-vb/_static/image12.png)](displaying-data-with-the-objectdatasource-vb/_static/image11.png)

**Figura 5**: scegliere il metodo per restituire i dati dalla scheda selezionare ([fare clic per visualizzare l'immagine ingrandita](displaying-data-with-the-objectdatasource-vb/_static/image13.png))


## <a name="configure-the-objectdatasource-manually"></a>Configurare manualmente ObjectDataSource

Configurazione guidata origine dati ObjectDataSource offre un modo rapido per specificare l'oggetto che viene utilizzato e associare vengono richiamati i metodi dell'oggetto. Tuttavia, è possibile configurare ObjectDataSource mediante le relative proprietà, tramite la finestra proprietà o direttamente nel markup dichiarativo. Impostare semplicemente il `TypeName` proprietà per il tipo dell'oggetto sottostante da utilizzare e `SelectMethod` al metodo da richiamare quando il recupero dei dati.


[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample1.aspx)]

Anche se si preferisce la configurazione guidata origine dati può accadere quando è necessario configurare manualmente ObjectDataSource, come la procedura guidata elenca solo le classi create dallo sviluppatore. Se si desidera associare ObjectDataSource a una classe in .NET Framework, ad esempio il [la classe di appartenenza](https://msdn.microsoft.com/en-us/library/system.web.security.membership.aspx), per accedere alle informazioni sull'account utente, o [classe Directory](https://msdn.microsoft.com/en-us/library/system.io.directory.aspx) per funzionare con informazioni sul file system è necessario impostare manualmente le proprietà di ObjectDataSource.

## <a name="step-2-adding-a-data-web-control-and-binding-it-to-the-objectdatasource"></a>Passaggio 2: Aggiunta di un controllo Web di dati e associarlo a ObjectDataSource

Una volta ObjectDataSource è stato aggiunto alla pagina e configurato, si è pronti per aggiungere dati a controlli Web alla pagina per visualizzare i dati restituiti da ObjectDataSource `Select` metodo. I dati di controllo Web possono essere associati a ObjectDataSource; Diamo un'occhiata di visualizzazione dei dati di ObjectDataSource in un controllo GridView, DetailsView e FormView.

## <a name="binding-a-gridview-to-the-objectdatasource"></a>Associazione di un controllo GridView a ObjectDataSource

Aggiungere un controllo GridView dalla casella degli strumenti `SimpleDisplay.aspx`dell'area di progettazione. Smart tag del controllo GridView, scegliere il controllo ObjectDataSource che aggiunto nel passaggio 1. Verrà creato automaticamente un BoundField in GridView per ciascuna proprietà restituita in base ai dati da ObjectDataSource `Select` metodo (in particolare, le proprietà definite nel DataTable dei prodotti).


[![Un controllo GridView è stato aggiunto alla pagina e associato a ObjectDataSource](displaying-data-with-the-objectdatasource-vb/_static/image15.png)](displaying-data-with-the-objectdatasource-vb/_static/image14.png)

**Figura 6**: A GridView è stato aggiunto alla pagina e associato a ObjectDataSource ([fare clic per visualizzare l'immagine ingrandita](displaying-data-with-the-objectdatasource-vb/_static/image16.png))


È quindi possibile personalizzare, ridisporre o rimuovere i BoundField di GridView facendo clic sull'opzione di modifica colonne dallo smart tag.


[![Gestire i BoundField di GridView mediante la finestra di dialogo Modifica colonne](displaying-data-with-the-objectdatasource-vb/_static/image18.png)](displaying-data-with-the-objectdatasource-vb/_static/image17.png)

**Figura 7**: gestione BoundField tramite la modifica colonne la del controllo GridView finestra di dialogo ([fare clic per visualizzare l'immagine ingrandita](displaying-data-with-the-objectdatasource-vb/_static/image19.png))


È opportuno modificare BoundField di GridView, la rimozione di `ProductID`, `SupplierID`, `CategoryID`, `QuantityPerUnit`, `UnitsInStock`, `UnitsOnOrder`, e `ReorderLevel` BoundField. Semplicemente selezionare le BoundField dall'elenco in basso a sinistra e fare clic sul pulsante di eliminazione (X rossa) per rimuoverle. Successivamente, ridisporre il BoundField in modo che il `CategoryName` e `SupplierName` precedano il `UnitPrice` BoundField selezionando questi BoundField e facendo clic sulla freccia verso l'alto. Impostare il `HeaderText` le proprietà dei restanti BoundField per `Products`, `Category`, `Supplier`, e `Price`, rispettivamente. Chiedere quindi il `Price` BoundField formattati come valuta impostando il BoundField `HtmlEncode` la proprietà su False e il relativo `DataFormatString` proprietà `{0:c}`. Infine, allineare in orizzontale il `Price` a destra e `Discontinued` casella di controllo nel centro tramite il `ItemStyle` / `HorizontalAlign` proprietà.


[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample2.aspx)]


[![Sono stati personalizzati BoundField di GridView](displaying-data-with-the-objectdatasource-vb/_static/image21.png)](displaying-data-with-the-objectdatasource-vb/_static/image20.png)

**Figura 8**: BoundField sono state personalizzate il GridView ([fare clic per visualizzare l'immagine ingrandita](displaying-data-with-the-objectdatasource-vb/_static/image22.png))


## <a name="using-themes-for-a-consistent-look"></a>Utilizzo dei temi per un aspetto coerente

Queste esercitazioni cercano di rimuovere le impostazioni di stile a livello di controllo, utilizzando invece fogli di stile CSS definita in un file esterno quando possibile. Il `Styles.css` contiene file `DataWebControlStyle`, `HeaderStyle`, `RowStyle`, e `AlternatingRowStyle` classi CSS che devono essere utilizzate per determinare l'aspetto dei dati di controlli Web utilizzati in queste esercitazioni. A tale scopo, è possibile impostare il controllo GridView `CssClass` proprietà `DataWebControlStyle`e il relativo `HeaderStyle`, `RowStyle`, e `AlternatingRowStyle` proprietà `CssClass` proprietà conseguenza.

Se si imposta questi `CssClass` proprietà a livello di controllo Web è necessario ricordare di impostare in modo esplicito i valori di queste proprietà per i dati di ogni controllo aggiunto per le esercitazioni di Web. Un approccio più gestibile consiste nel definire le proprietà correlate CSS predefinite per GridView, DetailsView, e FormView utilizzando un tema. Un tema è una raccolta di impostazioni delle proprietà a livello di controllo, immagini e le classi CSS che possono essere applicate alle pagine dei siti per applicare un aspetto comune.

Il tema non includerà immagini o file CSS (lasciare il foglio di stile `Styles.css` come-è definito nella cartella radice dell'applicazione web), ma includerà due interfacce. Un'interfaccia è un file che definisce le proprietà predefinite per un controllo Web. In particolare, è necessario un file di interfaccia per i controlli GridView e DetailsView, che indica il valore predefinito `CssClass`-le proprietà correlate.

Per iniziare, aggiungere un nuovo File di interfaccia al progetto denominato `GridView.skin` facendo clic sul nome del progetto in Esplora soluzioni e scegliendo Aggiungi nuovo elemento.


[![Aggiungere un File di interfaccia denominato GridView.skin](displaying-data-with-the-objectdatasource-vb/_static/image24.png)](displaying-data-with-the-objectdatasource-vb/_static/image23.png)

**Figura 9**: aggiungere un File di interfaccia denominato `GridView.skin` ([fare clic per visualizzare l'immagine ingrandita](displaying-data-with-the-objectdatasource-vb/_static/image25.png))


File di interfaccia devono essere inseriti in un tema, che si trovano nel `App_Themes` cartella. Poiché tale cartella non sono ancora, Visual Studio chiederà di crearlo automaticamente quando si aggiunge la prima interfaccia. Fare clic su Sì per creare il `App_Theme` cartella e inserire il nuovo `GridView.skin` file non esiste.


[![Consente di creare la cartella App_Theme Visual Studio](displaying-data-with-the-objectdatasource-vb/_static/image27.png)](displaying-data-with-the-objectdatasource-vb/_static/image26.png)

**Figura 10**: consente di Visual Studio crea il `App_Theme` cartella ([fare clic per visualizzare l'immagine ingrandita](displaying-data-with-the-objectdatasource-vb/_static/image28.png))


Verrà creato un nuovo tema nel `App_Themes` cartella denominata GridView con il file di interfaccia `GridView.skin`.


![Il tema di GridView è stato aggiunto alla cartella App_Theme](displaying-data-with-the-objectdatasource-vb/_static/image29.png)

**Figura 11**: il tema di GridView è stato aggiunto il `App_Theme` cartella


Rinominare il tema GridView in DataWebControls (pulsante destro del mouse sulla cartella GridView nel `App_Theme` cartella e scegliere Rinomina). Quindi, immettere il seguente markup nel `GridView.skin` file:


[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample3.aspx)]

Definisce le proprietà predefinite per il `CssClass`-le proprietà correlate per qualsiasi GridView in tutte le pagine che usa il tema DataWebControls. Aggiungere un'altra interfaccia per DetailsView, un controllo Web che verrà usato al più presto di dati. Aggiungere una nuova interfaccia per il tema DataWebControls denominato `DetailsView.skin` e aggiungere il markup seguente:


[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample4.aspx)]

Con il nostro tema definito, l'ultimo passaggio consiste per applicare il tema alla nostra pagina ASP.NET. In base a una pagina per pagina o per tutte le pagine in un sito Web, è possibile applicare un tema. Si usa questo tema per tutte le pagine nel sito Web. A tale scopo, aggiungere il markup seguente per `Web.config`del `<system.web>` sezione:


[!code-xml[Main](displaying-data-with-the-objectdatasource-vb/samples/sample5.xml)]

Questo è tutto qui! Il `styleSheetTheme` impostazione indica che le proprietà specificate nel tema devono *non* eseguire l'override di proprietà specificate a livello di controllo. Per specificare che le impostazioni del tema non devono escludere le impostazioni di controllo, utilizzare il `theme` attributo al posto di `styleSheetTheme`; Purtroppo, le impostazioni del tema non vengono visualizzati nella visualizzazione progettazione di Visual Studio. Fare riferimento a [temi e interfacce Panoramica](https://msdn.microsoft.com/en-us/library/ykzx33wh.aspx) e [sul lato Server stili utilizzando temi](https://quickstarts.asp.net/quickstartv20/aspnet/doc/themes/stylesheettheme.aspx) per ulteriori informazioni su temi e interfacce; vedere [procedura: applicare temi ASP.NET](https://msdn.microsoft.com/en-us/library/0yy5hxdk%28VS.80%29.aspx) per ulteriori informazioni su configurazione di una pagina per l'utilizzo di un tema.


[![GridView Visualizza nome del prodotto, categoria, fornitore, prezzo e le informazioni obsolete](displaying-data-with-the-objectdatasource-vb/_static/image31.png)](displaying-data-with-the-objectdatasource-vb/_static/image30.png)

**Figura 12**: GridView Visualizza nome del prodotto, categoria, fornitore, prezzo e non più disponibili informazioni ([fare clic per visualizzare l'immagine ingrandita](displaying-data-with-the-objectdatasource-vb/_static/image32.png))


## <a name="displaying-one-record-at-a-time-in-the-detailsview"></a>Visualizzazione di un Record alla volta nel controllo DetailsView.

GridView Visualizza una riga per ogni record restituiti dal controllo origine dati a cui è associato. Esistono casi, tuttavia, quando si può decidere di visualizzare un unico record o un solo record alla volta. Il [controllo DetailsView](https://msdn.microsoft.com/en-us/library/s3w1w7t4.aspx) offre questa funzionalità, il rendering come HTML `<table>` con due colonne e una riga per ogni colonna o una proprietà associata al controllo. È possibile considerare di DetailsView come un GridView con un singolo record ruotato di 90 gradi.

Per iniziare, aggiungere un controllo DetailsView *sopra* GridView in `SimpleDisplay.aspx`. Successivamente, associarlo allo stesso controllo ObjectDataSource di GridView. Come in GridView, un BoundField verrà aggiunto al controllo DetailsView per ogni proprietà nell'oggetto restituito da ObjectDataSource `Select` metodo. L'unica differenza è che i BoundField di DetailsView sono disposti orizzontalmente anziché verticalmente.


[![Aggiungere un controllo DetailsView alla pagina e associarlo a ObjectDataSource](displaying-data-with-the-objectdatasource-vb/_static/image34.png)](displaying-data-with-the-objectdatasource-vb/_static/image33.png)

**Figura 13**: aggiungere un controllo DetailsView alla pagina e associarlo a ObjectDataSource ([fare clic per visualizzare l'immagine ingrandita](displaying-data-with-the-objectdatasource-vb/_static/image35.png))


Ad esempio GridView, BoundField di DetailsView possono essere modificati per fornire una visualizzazione più personalizzata dei dati restituiti da ObjectDataSource. Nella figura 14 mostra DetailsView dopo i BoundField e `CssClass` proprietà sono state configurate per rendere l'aspetto simile all'esempio di GridView.


[![DetailsView Mostra un singolo Record](displaying-data-with-the-objectdatasource-vb/_static/image37.png)](displaying-data-with-the-objectdatasource-vb/_static/image36.png)

**Nella figura 14**: DetailsView Mostra un singolo Record ([fare clic per visualizzare l'immagine ingrandita](displaying-data-with-the-objectdatasource-vb/_static/image38.png))


Si noti che DetailsView visualizza solo il primo record restituito dall'origine dati. Per consentire all'utente di eseguire tutti i record, uno alla volta, è necessario attivare il paging in DetailsView. A tale scopo, tornare a Visual Studio e selezionare la casella di controllo Abilita Paging nello smart tag del controllo DetailsView.


[![Abilita il Paging nel controllo DetailsView](displaying-data-with-the-objectdatasource-vb/_static/image40.png)](displaying-data-with-the-objectdatasource-vb/_static/image39.png)

**Figura 15**: Abilita Paging nel controllo DetailsView ([fare clic per visualizzare l'immagine ingrandita](displaying-data-with-the-objectdatasource-vb/_static/image41.png))


[![Con il Paging è abilitato, il controllo DetailsView consente all'utente di visualizzare i prodotti](displaying-data-with-the-objectdatasource-vb/_static/image43.png)](displaying-data-with-the-objectdatasource-vb/_static/image42.png)

**Figura 16**: DetailsView abilitato il Paging, consente all'utente di visualizzare i prodotti ([fare clic per visualizzare l'immagine ingrandita](displaying-data-with-the-objectdatasource-vb/_static/image44.png))


Verranno fornite altre informazioni sulle esercitazioni di paging in futuro.

## <a name="a-more-flexible-layout-for-showing-one-record-at-a-time"></a>Un Layout più flessibile per visualizzare un Record alla volta

Il controllo DetailsView è alquanto rigido in modalità di visualizzazione ogni record restituito da ObjectDataSource. Si può decidere di una visualizzazione più flessibile dei dati. Ad esempio, anziché con il nome del prodotto, categoria, fornitore, prezzo e non più disponibili informazioni su una riga separata, potrebbe voler Mostra il nome del prodotto e il prezzo in un `<h4>` titolo, con le informazioni di categoria e il fornitore visualizzati sotto il nome e il prezzo di una dimensione inferiore. E potrebbe non desideriamo mostrare i nomi delle proprietà (prodotto, categoria e così via) accanto ai valori.

Il [controllo FormView](https://msdn.microsoft.com/en-US/library/fyf1dk77.aspx) fornisce questo livello di personalizzazione. Anziché utilizzare i campi (come GridView e DetailsView), FormView utilizza modelli che consentono una combinazione di controlli Web, il codice HTML statico, e [sintassi di associazione dati](http://www.15seconds.com/issue/040630.htm). Se si ha familiarità con il controllo Repeater da ASP.NET 1. x, è possibile considerare FormView come Repeater per visualizzare un singolo record.

Aggiungere un controllo FormView al `SimpleDisplay.aspx` area di progettazione della pagina. Vengono inizialmente FormView visualizzate come un blocco grigio, informare Microsoft che è necessario fornire, come minimo, il controllo `ItemTemplate`.


[![FormView deve includere un elemento ItemTemplate](displaying-data-with-the-objectdatasource-vb/_static/image46.png)](displaying-data-with-the-objectdatasource-vb/_static/image45.png)

**Figura 17**: il controllo FormView deve includere un `ItemTemplate` ([fare clic per visualizzare l'immagine ingrandita](displaying-data-with-the-objectdatasource-vb/_static/image47.png))


È possibile associare FormView direttamente a un controllo origine dati tramite smart tag del controllo FormView, verrà creato un valore predefinito `ItemTemplate` automaticamente (insieme a un `EditItemTemplate` e `InsertItemTemplate`, se il controllo di ObjectDatatSource `InsertMethod` e `UpdateMethod` vengono impostate proprietà). Tuttavia, per questo esempio si associare i dati al controllo FormView e specificare il relativo `ItemTemplate` manualmente. Iniziare con la configurazione del controllo FormView `DataSourceID` proprietà per il `ID` del controllo ObjectDataSource, `ObjectDataSource1`. Successivamente, creare il `ItemTemplate` affinché venga visualizzato, nome del prodotto e prezzo in un `<h4>` elemento e i nomi di categoria e spedizioniere sotto che in una dimensione inferiore.


[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample6.aspx)]


[![Il primo prodotto (Chai) viene visualizzato in un formato personalizzato](displaying-data-with-the-objectdatasource-vb/_static/image49.png)](displaying-data-with-the-objectdatasource-vb/_static/image48.png)

**Figura 18**: il primo prodotto (Chai) viene visualizzato in un formato personalizzato ([fare clic per visualizzare l'immagine ingrandita](displaying-data-with-the-objectdatasource-vb/_static/image50.png))


Il `<%# Eval(propertyName) %>` è riportata la sintassi di associazione dati. Il `Eval` metodo restituisce il valore della proprietà specificata per l'oggetto corrente da associare al controllo FormView. Consultare l'articolo di Alex Homer [semplificato ed estesi sintassi di associazione dati in ASP.NET 2.0](http://www.15seconds.com/issue/040630.htm) per ulteriori informazioni su vantaggi e svantaggi dell'associazione dati.

Analogamente a DetailsView, FormView mostra solo il primo record restituito da ObjectDataSource. È possibile abilitare il paging in FormView per consentire agli utenti di scorrere i prodotti uno alla volta.

## <a name="summary"></a>Riepilogo

L'accesso e visualizzazione dei dati da un livello di logica di Business può essere eseguite senza scrivere una riga di codice grazie al controllo ObjectDataSource di ASP.NET 2.0. ObjectDataSource richiama un metodo di una classe specificato e restituisce i risultati. Questi risultati possono essere visualizzati in un controllo Web associato a ObjectDataSource dei dati. In questa esercitazione è stato esaminato binding dei controlli GridView, DetailsView e FormView a ObjectDataSource.

Finora abbiamo visto solo come utilizzare ObjectDataSource per richiamare un metodo senza parametri, ma cosa accade se si desidera richiamare un metodo che prevede parametri di input, ad esempio il `ProductBLL` della classe `GetProductsByCategoryID(categoryID)`? Per chiamare un metodo che prevede uno o più parametri, dobbiamo configurare ObjectDataSource per specificare i valori per questi parametri. Vedremo come effettuare questa operazione nel nostro [esercitazione successiva](declarative-parameters-vb.md).

Buona programmazione!

## <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Creare controlli origine dati](https://msdn.microsoft.com/en-us/library/ms364049.aspx)
- [Esempi di GridView per ASP.NET 2.0](https://msdn.microsoft.com/en-us/library/aa479339.aspx)
- [Data Binding di sintassi in ASP.NET 2.0 estesi e semplificata](http://www.15seconds.com/issue/040630.htm)
- [Temi in ASP.NET 2.0](http://www.odetocode.com/Articles/423.aspx)
- [Stili sul lato server tramite i temi](https://quickstarts.asp.net/quickstartv20/aspnet/doc/themes/stylesheettheme.aspx)
- [Procedura: Applicare temi ASP.NET a livello di codice](https://msdn.microsoft.com/en-us/library/tx35bd89.aspx)

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore per questa esercitazione Hilton Giesenow. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Precedente](programmatically-setting-the-objectdatasource-s-parameter-values-cs.md)
[Successivo](declarative-parameters-vb.md)
