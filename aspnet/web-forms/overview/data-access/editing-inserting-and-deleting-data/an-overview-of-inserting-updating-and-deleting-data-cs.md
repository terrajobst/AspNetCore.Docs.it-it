---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs
title: Una panoramica di inserimento, aggiornamento ed eliminazione dei dati (c#) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione verrà illustrato come eseguire il mapping Insert () di un ObjectDataSource, Update (), e le classi, metodi Delete () per i metodi di livello BLL, nonché come configu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: b651dc58-93c7-4f83-a74e-3b99f6d60848
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 4e1b47782bd24824707266d1ed61e24789cc7a49
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369838"
---
<a name="an-overview-of-inserting-updating-and-deleting-data-c"></a>Una panoramica di inserimento, aggiornamento ed eliminazione dei dati (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'App di esempio](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_16_CS.exe) o [Scarica il PDF](an-overview-of-inserting-updating-and-deleting-data-cs/_static/datatutorial16cs1.pdf)

> In questa esercitazione verrà illustrato come eseguire il mapping Insert () di un ObjectDataSource, Update (), e i metodi Delete () per i metodi di livello BLL classi, nonché come configurare i controlli GridView, DetailsView e FormView per fornire funzionalità di modifica dei dati.


## <a name="introduction"></a>Introduzione

Tramite le varie esercitazioni precedenti abbiamo esaminato come visualizzare i dati in una pagina ASP.NET utilizzando i controlli GridView, DetailsView e FormView. Questi controlli lavorare semplicemente con i dati forniti a essi. In genere, questi controlli di accedere ai dati tramite l'uso di un controllo origine dati, ad esempio ObjectDataSource. Abbiamo visto come ObjectDataSource funge da proxy tra la pagina ASP.NET e i dati sottostanti. Quando un controllo GridView deve visualizzare i dati, viene richiamato il relativo ObjectDataSource `Select()` metodo, che a sua volta richiama un metodo dal nostro livello Business (LOGIC), che chiama un metodo in appropriato livello accesso dati (DAL) TableAdapter, che a sua volta invia una `SELECT` query al database Northwind.

Si tenga presente che quando abbiamo creato gli oggetti TableAdapter in nelle [la prima esercitazione](../introduction/creating-a-data-access-layer-cs.md), Visual Studio aggiunto automaticamente i metodi per l'inserimento, aggiornamento, e l'eliminazione dei dati da sottostante nella tabella di database. Inoltre, in [creazione di un livello di logica di Business](../introduction/creating-a-business-logic-layer-cs.md) metodi nel livello BLL che ha eseguito la chiamata è stato progettato in questi metodi di modifica dei dati.

Oltre al relativo `Select()` metodo, è inoltre ObjectDataSource `Insert()`, `Update()`, e `Delete()` metodi. Ad esempio il `Select()` (metodo), questi tre metodi possono essere mappati ai metodi in un oggetto sottostante. Se è configurato per inserire, aggiornare o eliminare dati, i controlli GridView, DetailsView e FormView forniscono un'interfaccia utente per la modifica dei dati sottostanti. Questa interfaccia utente chiama il `Insert()`, `Update()`, e `Delete()` metodi di ObjectDataSource, che quindi richiamano l'oggetto sottostante associato al metodi (vedere la figura 1).


[![Insert (), Update e Delete () metodi di ObjectDataSource agiscono da Proxy nel livello BLL](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image2.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image1.png)

**Figura 1**: The ObjectDataSource `Insert()`, `Update()`, e `Delete()` metodi agiscono da Proxy nel livello BLL ([fare clic per visualizzare l'immagine con dimensioni normali](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image3.png))


In questa esercitazione verrà illustrato come eseguire il mapping di ObjectDataSource `Insert()`, `Update()`, e `Delete()` metodi ai metodi delle classi nel livello BLL, nonché come configurare i controlli GridView, DetailsView e FormView per fornire la modifica dei dati funzionalità.

## <a name="step-1-creating-the-insert-update-and-delete-tutorials-web-pages"></a>Passaggio 1: Creazione di Insert, Update e Delete esercitazioni Web Pages

Prima di iniziare a esplorare come inserire, aggiornare ed eliminare dati, prima di tutto è opportuno creare le pagine ASP.NET nel progetto di sito Web che è necessario per questa esercitazione e quelli più avanti. Iniziare aggiungendo una nuova cartella denominata `EditInsertDelete`. Successivamente, aggiungere le seguenti pagine ASP.NET per quella cartella, assicurandosi di associare ogni pagina con il `Site.master` pagina master:

- `Default.aspx`
- `Basics.aspx`
- `DataModificationEvents.aspx`
- `ErrorHandling.aspx`
- `UIValidation.aspx`
- `CustomizedUI.aspx`
- `OptimisticConcurrency.aspx`
- `ConfirmationOnDelete.aspx`
- `UserLevelAccess.aspx`


![Aggiungere le pagine ASP.NET per le esercitazioni relative alla modifica dei dati](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image4.png)

**Figura 2**: aggiungere le pagine ASP.NET per le esercitazioni relative alla modifica dei dati


In altre cartelle, analogo a `Default.aspx` nella `EditInsertDelete` cartella elencherà le esercitazioni nella relativa sezione. Si tenga presente che il `SectionLevelTutorialListing.ascx` controllo utente fornisce questa funzionalità. Pertanto, aggiungere questo controllo utente da `Default.aspx` trascinandolo da Esplora soluzioni in visualizzazione di progettazione della pagina.


[![Aggiungere il controllo utente sectionleveltutoriallisting. ascx a default. aspx](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image6.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image5.png)

**Figura 3**: aggiungere i `SectionLevelTutorialListing.ascx` controllo utente da `Default.aspx` ([fare clic per visualizzare l'immagine con dimensioni normali](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image7.png))


Infine, aggiungere le pagine come voci per il `Web.sitemap` file. In particolare, aggiungere il markup seguente dopo la formattazione personalizzata `<siteMapNode>`:


[!code-xml[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample1.xml)]

Dopo aver aggiornato `Web.sitemap`, si consiglia di visualizzare il sito Web di esercitazioni tramite un browser. Il menu a sinistra ora include elementi per la modifica, inserimento ed eliminazione di esercitazioni.


![Mappa del sito include ora voci per la modifica, inserimento ed eliminazione esercitazioni](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image8.png)

**Figura 4**: la mappa del sito include ora voci per la modifica, inserimento ed eliminazione esercitazioni


## <a name="step-2-adding-and-configuring-the-objectdatasource-control"></a>Passaggio 2: Aggiunta e configurazione del controllo ObjectDataSource

Poiché il controllo GridView, DetailsView e FormView ognuno differiscono per le funzionalità di modifica dei dati e il layout, si esaminerà ognuna singolarmente. Anziché avere ogni controllo usando il proprio ObjectDataSource, tuttavia, solo creiamo un ObjectDataSource singolo che possono condividere tutti gli esempi di controllo tre.

Aprire il `Basics.aspx` pagina, trascinare un ObjectDataSource dalla casella degli strumenti nella finestra di progettazione e fare clic sul collegamento Configura origine dati dal suo smart tag. Poiché il `ProductsBLL` è l'unica classe BLL che fornisce la modifica, inserimento ed eliminazione di metodi, configurare ObjectDataSource per utilizzare questa classe.


[![Configurare ObjectDataSource per usare la classe ProductsBLL](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image10.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image9.png)

**Figura 5**: configurare ObjectDataSource per usare il `ProductsBLL` classe ([fare clic per visualizzare l'immagine con dimensioni normali](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image11.png))


Nella schermata successiva è possibile specificare quali metodi del `ProductsBLL` classe vengono mappati a ObjectDataSource `Select()`, `Insert()`, `Update()`, e `Delete()` selezionando la scheda appropriata e scegliendo il metodo dall'elenco a discesa. Figura 6, che dovrebbe avere familiarità con questo punto, viene eseguito il mapping di ObjectDataSource `Select()` metodo per il `ProductsBLL` della classe `GetProducts()` (metodo). Il `Insert()`, `Update()`, e `Delete()` metodi possono essere configurati da selezionando la scheda appropriata dall'elenco nella parte superiore.


[![Dispone di ObjectDataSource restituiscono tutti i prodotti](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image13.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image12.png)

**Figura 6**: dispone di ObjectDataSource restituiscono tutti i prodotti ([fare clic per visualizzare l'immagine con dimensioni normali](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image14.png))


Figure 7, 8 e 9 Mostra UPDATE, INSERT e DELETE di ObjectDataSource schede. Configurare queste schede, in modo che il `Insert()`, `Update()`, e `Delete()` metodi richiamano il `ProductsBLL` della classe `UpdateProduct`, `AddProduct`, e `DeleteProduct` metodi, rispettivamente.


[![Eseguire il mapping Update () metodo di ObjectDataSource al metodo UpdateProduct della classe Getproductsbycategoryid](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image16.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image15.png)

**Figura 7**: eseguire il mapping di ObjectDataSource `Update()` metodo per il `ProductBLL` della classe `UpdateProduct` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image17.png))


[![Eseguire il mapping Insert () metodo di ObjectDataSource della classe Getproductsbycategoryid AddProduct metodo](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image19.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image18.png)

**Figura 8**: eseguire il mapping di ObjectDataSource `Insert()` metodo per il `ProductBLL` Add della classe `Product` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image20.png))


[![Eseguire il mapping di metodo Delete () di ObjectDataSource per metodo della classe Getproductsbycategoryid DeleteProduct](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image22.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image21.png)

**Figura 9**: eseguire il mapping di ObjectDataSource `Delete()` metodo per il `ProductBLL` della classe `DeleteProduct` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image23.png))


Si è notato che gli elenchi a discesa nelle schede UPDATE, INSERT e DELETE dispone già di questi metodi selezionati. Si tratta di grazie all'uso dei dati di `DataObjectMethodAttribute` che decora i metodi di `ProducstBLL`. Ad esempio, il metodo DeleteProduct ha la firma seguente:


[!code-csharp[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample2.cs)]

Il `DataObjectMethodAttribute` attributo indica lo scopo di ogni metodo che specifica se è per la selezione, inserimento, l'aggiornamento, o l'eliminazione ed è o meno il valore predefinito. Se si omette questi attributi quando si creano classi BLL, si sarà necessario selezionare manualmente i metodi rispetto all'aggiornamento, inserire ed eliminare le schede.

Dopo aver verificato che l'appropriato `ProductsBLL` vengono eseguito il mapping di metodi di ObjectDataSource `Insert()`, `Update()`, e `Delete()` metodi, fare clic su Fine per completare la procedura guidata.

## <a name="examining-the-objectdatasources-markup"></a>Esaminare il Markup di ObjectDataSource

Dopo aver configurato ObjectDataSource mediante la procedura guidata, passare alla visualizzazione origine per esaminare il markup dichiarativo generato. Il `<asp:ObjectDataSource>` tag specifica l'oggetto sottostante e i metodi da richiamare. Esistono inoltre `DeleteParameters`, `UpdateParameters`, e `InsertParameters` che eseguire il mapping ai parametri di input per il `ProductsBLL` della classe `AddProduct`, `UpdateProduct`, e `DeleteProduct` metodi:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample3.aspx)]

ObjectDataSource includa un parametro per ognuno dei parametri di input per i metodi associati, proprio come un elenco di `SelectParameter` s è presente quando l'oggetto ObjectDataSource è configurato per chiamare un metodo di selezione che prevede un parametro di input (ad esempio `GetProductsByCategoryID(categoryID)`). Come si vedrà a breve, i valori per questi `DeleteParameters`, `UpdateParameters`, e `InsertParameters` vengono impostate automaticamente per il controllo GridView, DetailsView e FormView prima di richiamare ObjectDataSource `Insert()`, `Update()`, o `Delete()` metodo. Questi valori possono anche essere impostati a livello di codice in base alle esigenze, come vedremo in un'esercitazione futura.

Un effetto di usando la procedura guidata per configurare a ObjectDataSource consiste nel fatto che Visual Studio imposta la [proprietà OldValuesParameterFormatString](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.oldvaluesparameterformatstring(VS.80).aspx) a `original_{0}`. Valore di questa proprietà viene usato per includere i valori originali dei dati in fase di modifica e risulta utile in due scenari:

- Se, quando si modifica un record, gli utenti sono in grado di modificare il valore della chiave primaria. In questo caso, sia il nuovo valore di chiave primaria e il valore di chiave primaria originale deve essere forniti in modo che il record con il valore di chiave primaria originale può essere trovato e avere il valore aggiornato di conseguenza.
- Quando si usa la concorrenza ottimistica. La concorrenza ottimistica è una tecnica per garantire che due utenti simultanei non sovrascrivere le modifiche degli altri e l'argomento per un'esercitazione futura.

Il `OldValuesParameterFormatString` proprietà indica il nome dei parametri di input in update dell'oggetto sottostante e metodi di eliminazione per i valori originali. Questa proprietà e il relativo scopo più dettagliatamente tratteremo quando verrà esaminata la concorrenza ottimistica. Aprirla a questo punto, tuttavia, poiché i metodi del nostro BLL non sono prevedono i valori originali e pertanto è importante che è possibile rimuovere questa proprietà. Lasciando la `OldValuesParameterFormatString` proprietà è impostata su un valore qualsiasi diverso da quello predefinito (`{0}`) verrà generato un errore quando un controllo Web per dati tenta di richiamare ObjectDataSource `Update()` o `Delete()` metodi perché verrà ObjectDataSource prova a passare in entrambe le `UpdateParameters` o `DeleteParameters` specificati, nonché i parametri di valore originale.

Se questo non è molto chiaro a questo punto, non preoccuparti, esamineremo questa proprietà e l'utilità in un'esercitazione futura. Per ora, semplicemente essere determinati da rimuovere questa dichiarazione di proprietà interamente dalla sintassi dichiarativa o impostare il valore per il valore predefinito ({0}).

> [!NOTE]
> Se sufficiente cancellare i `OldValuesParameterFormatString` rimarranno nella sintassi dichiarativa per il valore della proprietà dalla finestra delle proprietà nella visualizzazione progettazione, la proprietà, ma essere impostata su una stringa vuota. Questa operazione, Sfortunatamente, verrà comunque comportare lo stesso problema indicato in precedenza. Pertanto, rimuovere la proprietà completamente dalla sintassi dichiarativa o, dalla finestra delle proprietà, imposta il valore per l'impostazione predefinita, `{0}`.


## <a name="step-3-adding-a-data-web-control-and-configuring-it-for-data-modification"></a>Passaggio 3: Aggiunta di un controllo Web per dati e configurarlo per la modifica dei dati

Una volta ObjectDataSource è stato aggiunto alla pagina e configurato, siamo pronti per aggiungere controlli Web dei dati per la pagina per visualizzare i dati e forniscono un mezzo per l'utente finale per modificarlo. Esamineremo il controllo GridView, DetailsView e FormView separatamente, come i controlli Web dei dati sono diversi nelle funzionalità di modifica dei dati e la configurazione.

Come vedremo nella parte restante di questo articolo, aggiunta molto semplice modifica, inserimento ed eliminazione di supporto tramite il controllo GridView, DetailsView e FormView è davvero semplice come il controllo di un paio di caselle di controllo. Esistono molti sfaccettature e casi limite del mondo reale che rendono la compilazione di più complesso rispetto a semplicemente puntare e fare clic su tali funzionalità. Tuttavia, questa esercitazione è incentrata esclusivamente sul dimostra la funzionalità di modifica dei dati semplicistico. Nelle esercitazioni successive verranno esaminati i problemi che indubbiamente sorgeranno in un'impostazione del mondo reale.

## <a name="deleting-data-from-the-gridview"></a>Eliminazione di dati da GridView

Per iniziare, trascinare un controllo GridView dalla casella degli strumenti nella finestra di progettazione. Successivamente, è possibile associare ObjectDataSource a GridView selezionandolo dall'elenco a discesa nello smart tag del controllo GridView. A questo punto markup dichiarativo del controllo GridView sarà:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample4.aspx)]

Associazione di GridView a ObjectDataSource mediante relativo smart tag offre due vantaggi:

- BoundField e CheckBoxFields vengono creati automaticamente per ognuno dei campi restituiti da ObjectDataSource. Inoltre, i BoundField e della CampoCasellaDiControllo proprietà vengono impostate in base in base ai metadati del campo sottostante. Ad esempio, il `ProductID`, `CategoryName`, e `SupplierName` i campi sono contrassegnati come di sola lettura nel `ProductsDataTable` e pertanto non deve essere aggiornabile quando si modifica. Per supportare questo, questi BoundField [delle proprietà di sola lettura](https://msdn.microsoft.com/library/system.web.ui.webcontrols.boundfield.readonly(VS.80).aspx) sono impostati su `true`.
- Il [DataKeyNames proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.datakeynames(VS.80).aspx) viene assegnato a uno o più campi chiave primari dell'oggetto sottostante. Ciò è essenziale quando uso GridView per la modifica o eliminazione dei dati, come questa proprietà indica il campo (o set di campi) univoco che identifica ogni record. Per altre informazioni sul `DataKeyNames` proprietà, fare riferimento al [Master/dettaglio mediante un Master GridView selezionabile con un controllo DetailView per i dettagli](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) esercitazione.

Anche se può essere associato il controllo GridView a ObjectDataSource mediante la finestra proprietà o la sintassi dichiarativa per, in questo modo richiede è quindi necessario aggiungere manualmente i BoundField appropriato e `DataKeyNames` markup.

Il controllo GridView offre supporto predefinito per la modifica a livello di riga e l'eliminazione. Configurazione di un controllo GridView per supportare l'eliminazione aggiunge una colonna di pulsanti di eliminazione. Quando l'utente finale fa clic sul pulsante Elimina per una determinata riga, un postback previsioni e GridView esegue i passaggi seguenti:

1. ObjectDataSource `DeleteParameters` vengono assegnati valori
2. ObjectDataSource `Delete()` metodo viene richiamato, eliminare il record specificato
3. Il controllo GridView riassocia a ObjectDataSource richiamando relativo `Select()` (metodo)

I valori assegnati per il `DeleteParameters` sono i valori del `DataKeyNames` uno o più campi per la riga è stato fatto clic sul pulsante la cui eliminazione. Pertanto è fondamentale che un controllo GridView `DataKeyNames` proprietà sia impostato correttamente. Se è presente, il `DeleteParameters` verrà assegnato un `null` valore nel passaggio 1, che a sua volta non comporterà uno qualsiasi eliminata i record nel passaggio 2.

> [!NOTE]
> Il `DataKeys` raccolta viene archiviata nello stato del controllo GridView s, vale a dire che il `DataKeys` valori verranno memorizzati durante il postback anche se lo stato di visualizzazione GridView s è stato disabilitato. Tuttavia, è molto importante che lo stato di visualizzazione rimane abilitato per GridView che supportano la modifica o eliminazione (comportamento predefinito). Se si imposta la s GridView `EnableViewState` proprietà `false`, la modifica ed eliminazione comportamento funzionerà correttamente per un singolo utente, ma se sono presenti utenti simultanei, l'eliminazione dei dati, esiste la possibilità che questi utenti simultanei potrebbero accidentalmente eliminazione o modifica di record che essi t prevede. Vedere il post di blog [avviso: problema di concorrenza con ASP.NET 2.0 GridViews/DetailsView/FormViews tale supporto di modifica e/o l'eliminazione e il cui stato di visualizzazione è disabilitato](http://scottonwriting.net/sowblog/archive/2006/10/03/163215.aspx), per altre informazioni.


Questo avviso stesso si applica anche a maggior facilità ai DetailsView e FormViews.

Per aggiungere funzionalità di eliminazione in un controllo GridView, è sufficiente passare al relativo smart tag e selezionare la casella di controllo Abilita eliminazione.


![Verificare di abilitare l'eliminazione di casella di controllo](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image24.png)

**Figura 10**: verificare di abilitare l'eliminazione di casella di controllo


Selezionare la casella di controllo Abilita eliminazione dallo smart tag aggiunge un CommandField a GridView. Il CommandField esegue il rendering di una colonna GridView con i pulsanti per l'esecuzione di uno o più delle seguenti attività: selezionare un record, un record di modifica ed eliminazione di un record. Abbiamo visto in precedenza CommandField in azione con la selezione di record nel [Master/dettaglio mediante un Master GridView selezionabile con un controllo DetailView per i dettagli](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) esercitazione.

Il CommandField contiene un numero di `ShowXButton` proprietà che indicano quali serie di pulsanti vengono visualizzati nella finestra di CommandField. Selezionando la casella di controllo un CommandField Abilita eliminazione il cui `ShowDeleteButton` è di proprietà `true` è stato aggiunto alla raccolta di colonne di GridView.


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample5.aspx)]

A questo punto, si creda o No, le operazioni con l'aggiunta di supporto per l'eliminazione a GridView. Come illustrato nella figura 11, quando visitando questa pagina tramite un browser una colonna di pulsanti di eliminazione è presente.


[![Il CommandField aggiunge una colonna di pulsanti di eliminazione](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image26.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image25.png)

**Figura 11**: il CommandField aggiunge una colonna di pulsanti Elimina ([fare clic per visualizzare l'immagine con dimensioni normali](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image27.png))


Se stai creando questa esercitazione sin dall'inizio per conto proprio, durante il test di questa pagina facendo clic sul pulsante Elimina genererà un'eccezione. Continuare a leggere per informazioni su per quanto riguarda il motivo per cui sono state generate queste eccezioni e come risolverli.

> [!NOTE]
> Se state procedendo con download che accompagna questa esercitazione, questi problemi sono già stati presi in considerazione per. Tuttavia, ti invitiamo a leggere i dettagli riportati di seguito per consentire di identificare i problemi che potrebbero verificarsi e soluzioni alternative adatte.


Se, durante il tentativo di eliminare un prodotto, si verifica un'eccezione il cui messaggio è simile a "*ObjectDataSource 'ObjectDataSource1' non è stato possibile trovare un metodo non generico 'DeleteProduct' che dispone di parametri: productID, originale\_ ProductID*, "si è probabilmente dimenticata rimuovere il `OldValuesParameterFormatString` proprietà da ObjectDataSource. Con il `OldValuesParameterFormatString` proprietà specificata, ObjectDataSource tenta di passare in entrambe `productID` e `original_ProductID` parametri di input di `DeleteProduct` (metodo). `DeleteProduct`, tuttavia, accetta solo un singolo parametro di input, di conseguenza l'eccezione. Rimozione di `OldValuesParameterFormatString` proprietà (o impostarlo su `{0}`) indica a ObjectDataSource per non prova a passare nel parametro di input originale.


[![Assicurarsi che la proprietà OldValuesParameterFormatString è stata cancellata](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image29.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image28.png)

**Figura 12**: verificare che il `OldValuesParameterFormatString` proprietà è stata cancellata Out ([fare clic per visualizzare l'immagine con dimensioni normali](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image30.png))


Anche se è stato rimosso il `OldValuesParameterFormatString` proprietà, è comunque verrà generata un'eccezione durante il tentativo di eliminare un prodotto con il messaggio: "*istruzione DELETE in conflitto con il vincolo di riferimento ' FK\_ordine\_dettagli \_Prodotti*. " Il database di Northwind contiene un vincolo di chiave esterna tra le `Order Details` e `Products` tabella, il che significa che un prodotto non può essere eliminato dal sistema se sono presenti uno o più record corrispondente nei `Order Details` tabella. Poiché tutti i prodotti nel database Northwind ha almeno un record in `Order Details`, non è possibile eliminare qualsiasi prodotto finché non viene eliminato prima di tutto i record di dettagli degli ordini associati del prodotto.


[![Un vincolo Foreign Key impedisce l'eliminazione dei prodotti](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image32.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image31.png)

**Figura 13**: un vincolo di chiave esterna impedisce l'eliminazione di prodotti ([fare clic per visualizzare l'immagine con dimensioni normali](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image33.png))


Per questa esercitazione, è possibile semplicemente eliminare tutti i record dal `Order Details` tabella. In un'applicazione reale è necessario uno:

- Dispone di un'altra schermata per gestire le informazioni sui dettagli ordine
- Aumentare il `DeleteProduct` per includere la logica per eliminare i dettagli dell'ordine del prodotto specificata (metodo)
- Modificare la query SQL utilizzata dal TableAdapter per includere l'eliminazione dei dettagli dell'ordine del prodotto specificato

È possibile semplicemente eliminare tutti i record dal `Order Details` per aggirare il vincolo di chiave esterna nella tabella. Passare a Esplora Server in Visual Studio, fare clic su di `NORTHWND.MDF` nodo e scegliere Nuova Query. Quindi, nella finestra query eseguire l'istruzione SQL seguente: `DELETE FROM [Order Details]`


[![Eliminare tutti i record dalla tabella Dettagli ordine](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image35.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image34.png)

**Figura 14**: Elimina tutti i record dal `Order Details` tabella ([fare clic per visualizzare l'immagine con dimensioni normali](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image36.png))


Dopo aver cancellato di `Order Details` tabella facendo clic sul pulsante Elimina eliminerà il prodotto senza errori. Se facendo clic sul pulsante Elimina non comporta l'eliminazione del prodotto, verificare che il controllo GridView `DataKeyNames` viene impostata per il campo di chiave primaria (`ProductID`).

> [!NOTE]
> Quando si fa clic sul pulsante Elimina un postback previsioni e il record viene eliminato. Ciò può essere pericoloso poiché è facile accidentalmente fare clic sul pulsante di eliminazione della riga errato. In un'esercitazione futura vedremo come aggiungere una conferma lato client quando si elimina un record.


## <a name="editing-data-with-the-gridview"></a>Modifica dei dati con GridView

Con l'eliminazione, il controllo GridView offre inoltre supporto incorporato di modifica a livello di riga. Configurazione di un controllo GridView per supportare la modifica aggiunge una colonna di pulsanti di modifica. Dal punto di vista dell'utente finale, facendo clic su cause di pulsante di modifica di una riga quella riga diventi modificabili, attivare le celle nelle caselle di testo contenente i valori esistenti e sostituendo i pulsanti Annulla e il pulsante di modifica con l'aggiornamento. Dopo aver apportato le modifiche desiderate, l'utente finale può scegliere il pulsante Update per salvare le modifiche o il pulsante Annulla per rimuoverle. In entrambi i casi, dopo aver fatto clic aggiornamento o Annulla GridView restituisce lo stato di pre-editing.

Dal nostro punto di vista come sviluppatore della pagina, quando l'utente finale fa clic sul pulsante Modifica per una determinata riga, un postback previsioni e GridView esegue i passaggi seguenti:

1. Il controllo GridView `EditItemIndex` proprietà viene assegnato all'indice della riga di cui è stato fatto clic sul pulsante la cui modifica
2. Il controllo GridView riassocia a ObjectDataSource richiamando relativo `Select()` (metodo)
3. L'indice di riga che corrisponde il `EditItemIndex` viene eseguito il rendering in "modalità di modifica". In questa modalità, il pulsante di modifica viene sostituito da pulsanti Annulla e aggiornamento e BoundField cui `ReadOnly` le proprietà sono False (impostazione predefinita) vengono sottoposti a rendering come controlli Web nella casella di testo, la cui proprietà `Text` proprietà vengono assegnate ai valori dei campi dati.

A questo punto il markup viene restituito al browser, consentendo all'utente finale di apportare modifiche ai dati della riga. Quando l'utente fa clic sul pulsante di aggiornamento, si verifica un postback e GridView esegue i passaggi seguenti:

1. ObjectDataSource `UpdateParameters` valori vengono assegnati i valori immessi dall'utente finale nell'interfaccia di modifica del controllo GridView
2. ObjectDataSource `Update()` metodo viene richiamato, l'aggiornamento del record specificato
3. Il controllo GridView riassocia a ObjectDataSource richiamando relativo `Select()` (metodo)

Valori di chiave primaria assegnati per il `UpdateParameters` nel passaggio 1 provengono dai valori specificati nel `DataKeyNames` proprietà, mentre i valori di chiave non primaria provengono dal testo nei controlli Web nella casella di testo per la riga modificata. Come con l'eliminazione, è fondamentale che un controllo GridView `DataKeyNames` proprietà sia impostato correttamente. Se è presente, il `UpdateParameters` valore di chiave primaria verrà assegnato un `null` valore nel passaggio 1, che a sua volta non comporterà uno aggiornati i record nel passaggio 2.

Selezionando semplicemente la casella di controllo Abilita modifica nello smart tag del controllo GridView, è possibile attivare la funzionalità di modifica.


![Verificare di abilitare la casella di controllo di modifica](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image37.png)

**Figura 15**: verificare di abilitare la casella di controllo di modifica


Controllare la casella di controllo Abilita modifica aggiungerà un CommandField (se necessario) e set relativo `ShowEditButton` proprietà `true`. Come abbiamo visto in precedenza, il CommandField contiene un numero di `ShowXButton` proprietà che indicano quali serie di pulsanti vengono visualizzati nella finestra di CommandField. Selezionare la casella di controllo Abilita modifica aggiunge il `ShowEditButton` proprietà per il CommandField esistente:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample6.aspx)]

Questo è tutto ciò c'è da aggiunta rudimentale supporto di modifica. Come illustrato di seguito Figure16, l'interfaccia di modifica è piuttosto elementare ogni BoundField cui `ReadOnly` è impostata su `false` (predefinito) viene eseguito il rendering come una casella di testo. Questa categoria include campi quali `CategoryID` e `SupplierID`, quali sono le chiavi con altre tabelle.


[![Facendo clic sul pulsante di modifica s Chai consente di visualizzare la riga in modalità di modifica](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image39.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image38.png)

**Figura 16**: s Chai facendo clic sul pulsante Modifica consente di visualizzare la riga in modalità di modifica ([fare clic per visualizzare l'immagine con dimensioni normali](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image40.png))


Oltre a chiedere agli utenti di modificare direttamente i valori di chiave esterna, l'interfaccia dell'interfaccia modifica risulta mancante nei modi seguenti:

- Se l'utente immette una `CategoryID` oppure `SupplierID` che non esiste nel database, il `UPDATE` che violano un vincolo di chiave esterna, causando un'eccezione da generare.
- L'interfaccia di modifica non include alcuna convalida. Se non si specifica un valore obbligatorio (ad esempio `ProductName`), oppure immettere un valore stringa in cui è previsto un valore numerico (ad esempio, immettere "Troppe operazioni". nel `UnitPrice` nella casella di testo), verrà generata un'eccezione. Un'esercitazione futura verrà esaminate come aggiungere i controlli di convalida all'interfaccia utente di modifica.
- Attualmente *tutti* campi prodotto che non sono di sola lettura devono essere incluso nel controllo GridView. Se dovessimo per rimuovere un campo da GridView, pronunciare `UnitPrice`, quando l'aggiornamento dei dati di GridView non imposterebbe il `UnitPrice` `UpdateParameters` valore, che potrebbe modificare il record di database `UnitPrice` a un `NULL` valore. Analogamente, se un campo obbligatorio, ad esempio `ProductName`, viene rimosso da GridView, l'aggiornamento avrà esito negativo con lo stesso "*colonna"NomeProdotto"non consente valori null*" eccezione indicato in precedenza.
- La modifica dell'interfaccia formattazione lascia un possono essere notevolmente. Il `UnitPrice` viene visualizzato con quattro cifre decimali. Idealmente il `CategoryID` e `SupplierID` valori conterrebbe controlli DropDownList cui sono elencate le categorie e i fornitori nel sistema.

Questi sono tutti i difetti che avremo imparare a sopportarne per ora, ma verrà risolto in esercitazioni future.

## <a name="inserting-editing-and-deleting-data-with-the-detailsview"></a>Inserimento, modifica ed eliminazione dei dati con il controllo DetailsView

Come illustrato nelle esercitazioni precedenti, il controllo DetailsView visualizza un record alla volta e, come per GridView, consente la modifica e l'eliminazione del record attualmente visualizzato. Sia l'esperienza dell'utente finale con modifica e l'eliminazione di elementi da un controllo DetailsView e il flusso di lavoro sul lato di ASP.NET è identico a quello di GridView. In cui il controllo DetailsView è diverso da GridView è che offre anche supporto predefinito di inserimento.

Per illustrare la funzionalità di modifica dei dati di GridView, iniziare aggiungendo un controllo DetailsView per la `Basics.aspx` pagina sopra il controllo GridView esistente e associarlo a ObjectDataSource esistente tramite smart tag di DetailsView. Successivamente, cancellare il contenuto di DetailsView `Height` e `Width` proprietà e selezionare l'opzione attiva Paging nello smart tag. Per abilitare la modifica, inserimento ed eliminazione di supporto, è sufficiente selezionare le caselle di controllo Abilita modifica, inserimento di abilitare e Abilita eliminazione nello smart tag.


![Configurare il controllo DetailsView per il supporto di modifica, inserimento ed eliminazione](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image41.png)

**Figura 17**: configurare il controllo DetailsView per il supporto di modifica, inserimento ed eliminazione


Come in GridView, aggiunta di modifica, inserimento o eliminazione di supporto aggiunge un CommandField a DetailsView, come illustrato nella sintassi dichiarativa per il seguente:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample7.aspx)]

Si noti che per il controllo DetailsView di CommandField visualizzata alla fine della raccolta di colonne per impostazione predefinita. Poiché i DetailsView campi vengono visualizzati come righe, il CommandField viene visualizzato come una riga con Insert, modificare ed eliminare i pulsanti nella parte inferiore del controllo DetailsView.


[![Configurare il controllo DetailsView per il supporto di modifica, inserimento ed eliminazione](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image43.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image42.png)

**Figura 18**: configurare il DetailsView alla assistenza modifica, inserimento ed eliminazione ([fare clic per visualizzare l'immagine con dimensioni normali](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image44.png))


Facendo clic sul pulsante Elimina viene avviata la stessa sequenza di eventi come per GridView: un postback; seguita da DetailsView popolamento ObjectDataSource `DeleteParameters` base il `DataKeyNames` valori; e completato con una chiamata di ObjectDataSource relativo `Delete()` metodo, che rimuove effettivamente il prodotto dal database. La modifica nel controllo DetailsView. funziona anche in modo identico a quello di GridView.

Per l'inserimento, l'utente finale viene presentato con un nuovo pulsante che, quando si fa clic, viene eseguito il rendering DetailsView in "modalità di inserimento". Con la "modalità di inserimento" viene sostituito il nuovo pulsante solo questi BoundField e i pulsanti Inserisci e Annulla il cui `InsertVisible` è impostata su `true` (predefinito) vengono visualizzati. Tali campi identificati come campi a incremento automatico, ad esempio `ProductID`, hanno loro [proprietà InsertVisible](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datacontrolfield.insertvisible(VS.80).aspx) impostato su `false` durante l'associazione all'origine dati tramite lo smart tag di DetailsView.

Quando si associa un'origine dati a un controllo DetailsView tramite lo smart tag, Visual Studio imposta la `InsertVisible` proprietà `false` solo per i campi a incremento automatico. I campi di sola lettura, ad esempio `CategoryName` e `SupplierName`, verrà visualizzato nell'interfaccia utente di "modalità di inserimento", a meno che loro `InsertVisible` viene impostata in modo esplicito su `false`. Si consiglia di impostare questi due campi `InsertVisible` proprietà `false`, tramite la sintassi dichiarativa DetailsView o i campi di modifica collegamento nello smart tag. Figura 19 mostra l'impostazione di `InsertVisible` proprietà `false` facendo clic su Modifica campi collegare.


[![Northwind Traders offre ora Acme tè](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image46.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image45.png)

**Figura 19**: Northwind Traders ora offre Acme tè ([fare clic per visualizzare l'immagine con dimensioni normali](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image47.png))


Dopo aver impostato la `InsertVisible` proprietà, vista la `Basics.aspx` pagina in un browser e fare clic sul pulsante nuovo. Figura 20 mostra DetailsView quando si aggiunge una nuovo bevande, tè Acme, per la linea di prodotti.


[![Northwind Traders offre ora Acme tè](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image49.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image48.png)

**Figura 20**: Northwind Traders ora offre Acme tè ([fare clic per visualizzare l'immagine con dimensioni normali](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image50.png))


Dopo aver immesso i dettagli per Acme tè e facendo clic sul pulsante di inserimento, un postback previsioni e viene aggiunto il nuovo record per il `Products` tabella di database. Poiché questo controllo DetailsView sono elencati i prodotti nell'ordine con cui esistono nella tabella di database, è necessario della pagina e l'ultimo prodotto per visualizzare il nuovo prodotto.


[![Dettagli per Acme tè](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image52.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image51.png)

**Figura 21**: i dettagli per tè Acme ([fare clic per visualizzare l'immagine con dimensioni normali](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image53.png))


> [!NOTE]
> DetailsView [proprietà CurrentMode](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.currentmode(VS.80).aspx) indica l'interfaccia viene visualizzata e può essere uno dei valori seguenti: `Edit`, `Insert`, o `ReadOnly`. Il [proprietà DefaultMode](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.defaultmode(VS.80).aspx) indica la modalità di DetailsView restituzione dopo una modifica o inserimento è stata completata ed è utile per la visualizzazione di un controllo DetailsView in modo permanente in modalità di modifica o modalità di inserimento.


Il punto e fare clic su inserimento e la modifica di funzionalità del controllo DetailsView subisce le stesse limitazioni di GridView: l'utente deve immettere esistente `CategoryID` e `SupplierID` valori tramite una casella di testo; non dispone di interfaccia qualsiasi logica di convalida; tutti i campi di prodotto che non ammettono `NULL` valori oppure non è un valore predefinito valore specificato a livello di database deve essere incluso nell'inserimento interfaccia e così via.

Le tecniche verranno esaminate per estensione e sul miglioramento interfaccia per la modifica del controllo GridView in futuro articoli è applicabile per il controllo DetailsView modifica e inserimento anche interfacce.

## <a name="using-the-formview-for-a-more-flexible-data-modification-user-interface"></a>Uso di FormView per un'interfaccia utente di modifica dei dati più flessibile

FormView offre supporto predefinito per l'inserimento, la modifica ed eliminazione dei dati, ma perché Usa i modelli anziché i campi non è possibile utilizzare per aggiungere i BoundField o il CommandField utilizzato dai controlli GridView e DetailsView per fornire i dati interfaccia di modifica. Al contrario, questa interfaccia, i controlli Web per la raccolta utente quando si aggiunge un nuovo elemento di input o modifica una esistente con nuovo, modificare, eliminare, inserimento, aggiornamento e i pulsanti Annulla deve essere aggiunti manualmente ai modelli appropriati. Fortunatamente, Visual Studio creerà automaticamente l'interfaccia necessaria quando si associa il controllo FormView a un'origine dati tramite l'elenco a discesa nel suo smart tag.

Per illustrare queste tecniche, iniziare aggiungendo un controllo FormView al `Basics.aspx` pagina e, dallo smart tag del controllo FormView, associarlo a ObjectDataSource già creato. Verrà generato un `EditItemTemplate`, `InsertItemTemplate`, e `ItemTemplate` per FormView con i controlli Web nella casella di testo per la raccolta di input dell'utente e i controlli pulsante Web per il nuovo, modificare, eliminare, inserimento, aggiornamento e i pulsanti Annulla. Inoltre, il FormView `DataKeyNames` viene impostata per il campo di chiave primaria (`ProductID`) dell'oggetto restituito da ObjectDataSource. Infine, selezionare l'opzione attiva Paging nello smart tag del controllo FormView.

Di seguito viene illustrato il markup dichiarativo per il controllo FormView `ItemTemplate` dopo aver associato il controllo FormView a ObjectDataSource. Per impostazione predefinita, ogni campo di prodotto di valori non booleani è associato ai `Text` proprietà di un controllo Web Label durante ogni campo del valore booleano (`Discontinued`) è associato il `Checked` proprietà di un controllo casella di controllo Web disabilitato. Affinché i pulsanti New, Edit e Delete attivare determinati comportamenti di FormView quando si fa clic, è fondamentale che loro `CommandName` impostare i valori `New`, `Edit`, e `Delete`, rispettivamente.


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample8.aspx)]

Figura 22 viene illustrato il controllo FormView `ItemTemplate` quando viene visualizzato tramite un browser. Ogni campo prodotto sia elencato con i pulsanti New, Edit e Delete nella parte inferiore.


[![ItemTemplate disporranno delle FormView Elenca ogni campo prodotto insieme ai nuovi, modificare ed eliminare i pulsanti](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image55.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image54.png)

**Figura 22**: The disporranno delle FormView `ItemTemplate` Elenca ogni prodotto campo lungo con New, Edit e Delete pulsanti ([fare clic per visualizzare l'immagine con dimensioni normali](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image56.png))


Come con GridView e DetailsView, fare clic sul pulsante Elimina o qualsiasi pulsante, LinkButton oppure ImageButton cui `CommandName` proprietà è impostata su Delete causa un postback, popola di ObjectDataSource `DeleteParameters` base di FormView `DataKeyNames`valore e richiama di ObjectDataSource `Delete()` (metodo).

Quando si fa clic sul pulsante Modifica un postback previsioni e i dati sono riassociati per il `EditItemTemplate`, che è responsabile del rendering l'interfaccia di modifica. Questa interfaccia include i controlli Web per la modifica dei dati con i pulsanti Annulla e Update. Il valore predefinito `EditItemTemplate` generato da Visual Studio contiene un'etichetta per tutti i campi a incremento automatico (`ProductID`), un controllo TextBox per ogni campo di valori non booleani e una casella di controllo per ogni campo di valore booleano. Questo comportamento è molto simile al BoundField generate automaticamente nei controlli GridView e DetailsView.

> [!NOTE]
> Un piccolo problema di generazione automatica del controllo FormView del `EditItemTemplate` è che esegue il rendering Web nella casella di testo per i campi che sono di sola lettura, ad esempio i controlli `CategoryName` e `SupplierName`. Si vedrà come considerare questo aspetto a breve.


Consente di controllare la casella di testo nel `EditItemTemplate` hanno loro `Text` proprietà associata al valore del campo dati corrispondente using *associazione dati bidirezionale*. Associazione dati bidirezionale, indicato da `<%# Bind("dataField") %>`, esegue l'associazione dati di entrambe durante l'associazione dei dati al modello e quando si popola i parametri di ObjectDataSource per l'inserimento o modifica di record. Ovvero, quando l'utente fa clic sul pulsante di modifica dal `ItemTemplate`, il `Bind()` metodo viene restituito il valore del campo dati specificato. Dopo che l'utente apporta le modifiche e fa clic sull'aggiornamento, i valori ha eseguito il postback che corrispondono ai campi di dati specificati mediante `Bind()` vengono applicati a ObjectDataSource `UpdateParameters`. In alternativa, l'associazione dati unidirezionale, identificato da `<%# Eval("dataField") %>`, solo recupera i valori di campo dati quando si associano dati al modello e *non* restituiscono i valori immessi dall'utente per i parametri dell'origine dati durante il postback.

Il markup dichiarativo seguente illustra il controllo FormView `EditItemTemplate`. Si noti che il `Bind()` metodo viene utilizzato in seguito la sintassi di associazione dati e i controlli Web di pulsante Annulla e aggiornamento con loro `CommandName` proprietà impostata di conseguenza.

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample9.aspx)]

Nostro `EditItemTemplate`, in questo momento, genererà un'eccezione generata se si prova a usarla. Il problema è che il `CategoryName` e `SupplierName` i campi vengono visualizzati come controlli Web nella casella di testo nel `EditItemTemplate`. È necessario modificare queste caselle di testo in etichette o rimuoverle completamente. Verrà ora è possibile eliminarle completamente dal `EditItemTemplate`.

Figura 23 è illustrata FormView in un browser dopo che è stato fatto clic di pulsante di modifica per Chai compare. Si noti che il `SupplierName` e `CategoryName` campi indicati nel `ItemTemplate` non sono più presenti, come abbiamo appena rimosso dal `EditItemTemplate`. Quando si fa clic sul pulsante Aggiorna FormView procede attraverso la stessa sequenza di passaggi come i controlli GridView e DetailsView.


[![Per impostazione predefinita EditItemTemplate Mostra ogni campo prodotto modificabile in una casella di testo o una casella di controllo](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image58.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image57.png)

**Figura 23**: per impostazione predefinita, il `EditItemTemplate` Mostra ogni prodotto campo modificabile come una casella di testo o una casella di controllo ([fare clic per visualizzare l'immagine con dimensioni normali](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image59.png))


Quando l'inserimento è fatto clic sul pulsante del controllo FormView `ItemTemplate` previsioni un postback. Tuttavia, nessun dato è associato a FormView perché viene aggiunto un nuovo record. Il `InsertItemTemplate` interfaccia include i controlli Web per l'aggiunta di un nuovo record con i pulsanti Inserisci e Annulla. Il valore predefinito `InsertItemTemplate` generato da Visual Studio contiene una casella di testo per ogni campo di valori non booleani e una casella di controllo per ogni campo di valore Boolean, simile a generata automaticamente `EditItemTemplate`dell'interfaccia. Dispongono i controlli TextBox loro `Text` proprietà associata al valore di loro campo di dati corrispondente tramite associazione dati bidirezionale.

Il markup dichiarativo seguente illustra il controllo FormView `InsertItemTemplate`. Si noti che il `Bind()` metodo viene utilizzato in seguito la sintassi di associazione dati e i controlli Web di pulsante Annulla e Insert con loro `CommandName` proprietà impostata di conseguenza.


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample10.aspx)]

È importante sottolineare che con generazione automatica del controllo FormView del `InsertItemTemplate`. In particolare, i controlli Web nella casella di testo vengono creati anche per i campi che sono di sola lettura, ad esempio `CategoryName` e `SupplierName`. Come con le `EditItemTemplate`, è necessario rimuovere queste caselle di testo dal `InsertItemTemplate`.

Figura 24 Mostra FormView in un browser quando si aggiunge un nuovo prodotto, Acme caffè. Si noti che il `SupplierName` e `CategoryName` campi indicati nel `ItemTemplate` non siano più presenti, come abbiamo appena rimosso. Quando viene scelto il pulsante di inserimento di FormView procede attraverso la stessa sequenza di passaggi come il controllo DetailsView, aggiungere un nuovo record per il `Products` tabella. Figura 25 Mostra i dettagli del prodotto Acme Coffee in FormView dopo che è stato inserito.


[![Il InsertItemTemplate determina l'interfaccia di inserimento del controllo FormView](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image61.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image60.png)

**Figura 24**: il `InsertItemTemplate` determina l'interfaccia di inserimento del controllo FormView ([fare clic per visualizzare l'immagine con dimensioni normali](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image62.png))


[![I dettagli per il nuovo prodotto, Acme caffè, vengono visualizzati in FormView](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image64.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image63.png)

**Figura 25**: il dettagli per il nuovo prodotto, Acme caffè, vengono visualizzati nel controllo FormView ([fare clic per visualizzare l'immagine con dimensioni normali](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image65.png))


Separando la sola lettura, modifica e inserimento di interfacce in tre modelli distinti, FormView consente un miglior grado di controllare queste interfacce da GridView e DetailsView.

> [!NOTE]
> Ad esempio DetailsView, FormView `CurrentMode` proprietà indica l'interfaccia viene visualizzata e il relativo `DefaultMode` proprietà indica la modalità di FormView torna poi al termine di una modifica o inserimento è stata completata.


## <a name="summary"></a>Riepilogo

In questa esercitazione abbiamo esaminato le nozioni di base di inserimento, modifica ed eliminazione dei dati usando il controllo GridView, DetailsView e FormView. Tutte e tre questi controlli offrono un certo livello di funzionalità di modifica di dati incorporato che può essere utilizzato senza scrivere una singola riga di codice in una pagina ASP.NET grazie a controlli Web dei dati e di ObjectDataSource. Tuttavia, il semplice e quindi le tecniche di rendering un piuttosto frail e interfaccia utente di modifica dati naive. Per fornire la convalida, inserire valori a livello di codice, gestire correttamente le eccezioni, personalizzare l'interfaccia utente e così via, sarà necessario fare affidamento su un insieme di tecniche che verranno analizzati tramite le esercitazioni più avanti.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [avanti](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md)
