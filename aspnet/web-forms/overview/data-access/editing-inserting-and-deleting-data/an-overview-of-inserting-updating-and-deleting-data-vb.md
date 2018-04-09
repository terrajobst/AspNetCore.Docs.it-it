---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb
title: Cenni preliminari di inserimento, aggiornamento ed eliminazione di dati (VB) | Documenti Microsoft
author: rick-anderson
description: In questa esercitazione si vedrà come eseguire il mapping di Insert () un ObjectDataSource, Update (), e le classi metodi Delete () per i metodi di BLL, nonché come eseguire la configu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 35b40b8f-2ca8-4ab3-9c19-f361a91a3647
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb
msc.type: authoredcontent
ms.openlocfilehash: db77d9ec5b0d4b27259023363e786b26fe736d7b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="an-overview-of-inserting-updating-and-deleting-data-vb"></a>Cenni preliminari di inserimento, aggiornamento ed eliminazione di dati (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare App di esempio](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_16_VB.exe) o [Scarica il PDF](an-overview-of-inserting-updating-and-deleting-data-vb/_static/datatutorial16vb1.pdf)

> In questa esercitazione si vedrà come eseguire il mapping di Insert () un ObjectDataSource, Update (), e le classi metodi Delete () per i metodi di BLL, nonché come configurare i controlli di GridView, DetailsView e FormView per fornire funzionalità di modifica dei dati.


## <a name="introduction"></a>Introduzione

Tramite le diverse esercitazioni precedenti abbiamo esaminato come visualizzare i dati in una pagina ASP.NET utilizzando i controlli di GridView, DetailsView e FormView. Questi controlli semplicemente utilizzano i dati forniti a essi. In genere, questi controlli accedere ai dati tramite l'utilizzo di un controllo origine dati, ad esempio ObjectDataSource. Abbiamo visto come ObjectDataSource funge da proxy tra la pagina ASP.NET e i dati sottostanti. Quando un controllo GridView deve visualizzare i dati, viene richiamato il relativo ObjectDataSource `Select()` metodo, che a sua volta richiama un metodo da questo livello Business (LOGIC), che chiama un metodo in appropriato livello accesso dati (DAL) TableAdapter, che a sua volta invia una `SELECT` query al database Northwind.

Tenere presente che, quando abbiamo creato gli oggetti TableAdapter in in [la prima esercitazione](../introduction/creating-a-data-access-layer-cs.md), Visual Studio aggiunti automaticamente i metodi per l'inserimento, aggiornamento, e tabella di database di eliminazione dei dati sottostante. Inoltre, in [la creazione di un livello di logica di Business](../introduction/creating-a-business-logic-layer-vb.md) in questi metodi di modifica DAL dati è stato progettato metodi BLL che ha chiamato verso il basso.

Oltre ai relativi `Select()` metodo, è inoltre ObjectDataSource `Insert()`, `Update()`, e `Delete()` metodi. Ad esempio il `Select()` (metodo), questi tre metodi possono essere mappati a metodi nell'oggetto sottostante. Quando è configurato per inserire, aggiornare o eliminare dati, i controlli di GridView, DetailsView e FormView forniscono un'interfaccia utente per la modifica dei dati sottostanti. Questa interfaccia utente chiama il `Insert()`, `Update()`, e `Delete()` metodi di ObjectDataSource, che quindi richiamano l'oggetto sottostante associata metodi (vedere la figura 1).


[![Insert (), Update () e i metodi di Delete () di ObjectDataSource fungono da Proxy in BLL](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image2.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image1.png)

**Figura 1**: il ObjectDataSource `Insert()`, `Update()`, e `Delete()` metodi fungere da Proxy in BLL ([fare clic per visualizzare l'immagine ingrandita](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image3.png))


In questa esercitazione si vedrà come eseguire il mapping di ObjectDataSource `Insert()`, `Update()`, e `Delete()` metodi per metodi delle classi BLL, nonché come configurare i controlli di GridView, DetailsView e FormView per fornire la modifica dei dati funzionalità.

## <a name="step-1-creating-the-insert-update-and-delete-tutorials-web-pages"></a>Passaggio 1: Creazione l'Insert, Update e Delete esercitazioni le pagine Web

Prima di iniziare l'esplorazione di inserire, aggiornare ed eliminare dati, innanzitutto è opportuno creare pagine ASP.NET nel progetto sito Web che è necessario per questa esercitazione, mentre quelle più avanti. Per iniziare, aggiungere una nuova cartella denominata `EditInsertDelete`. Successivamente, aggiungere le seguenti pagine ASP.NET a quella cartella, assicurandosi di associare ogni pagina con il `Site.master` pagina master:

- `Default.aspx`
- `Basics.aspx`
- `DataModificationEvents.aspx`
- `ErrorHandling.aspx`
- `UIValidation.aspx`
- `CustomizedUI.aspx`
- `OptimisticConcurrency.aspx`
- `ConfirmationOnDelete.aspx`
- `UserLevelAccess.aspx`


![Aggiungere le pagine ASP.NET per le esercitazioni relative alla modifica di dati](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image4.png)

**Figura 2**: aggiungere le pagine ASP.NET per le esercitazioni relative alla modifica dei dati


Come in altre cartelle, `Default.aspx` nel `EditInsertDelete` cartella elencherà le esercitazioni nella relativa sezione. Tenere presente che il `SectionLevelTutorialListing.ascx` controllo utente fornisce questa funzionalità. Pertanto, aggiungere il controllo utente `Default.aspx` trascinandolo da Esplora soluzioni in visualizzazione Progettazione della pagina.


[![Aggiungere il controllo utente SectionLevelTutorialListing.ascx Default.aspx](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image6.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image5.png)

**Figura 3**: aggiungere il `SectionLevelTutorialListing.ascx` controllo utente al `Default.aspx` ([fare clic per visualizzare l'immagine ingrandita](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image7.png))


Infine, aggiungere le pagine come voci per il `Web.sitemap` file. In particolare, aggiungere il markup seguente dopo la formattazione personalizzata `<siteMapNode>`:


[!code-xml[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample1.xml)]

Dopo aver aggiornato `Web.sitemap`, dedicare alcuni minuti per visualizzare il sito Web esercitazioni tramite un browser. Il menu a sinistra include elementi per la modifica, inserimento ed eliminazione di esercitazioni.


![Mappa del sito include ora le voci per la modifica, inserimento ed eliminazione di esercitazioni](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image8.png)

**Figura 4**: mappa del sito include ora le voci per la modifica, inserimento ed eliminazione di esercitazioni


## <a name="step-2-adding-and-configuring-the-objectdatasource-control"></a>Passaggio 2: Aggiunta e configurazione del controllo ObjectDataSource

Poiché il GridView, DetailsView e FormView ogni differiscono in termini di funzionalità di modifica dei dati e layout, esaminiamo ognuna singolarmente. Anziché avere ogni controllo utilizzando il proprio ObjectDataSource, tuttavia, solo creare un singolo ObjectDataSource che possono condividere tutti gli esempi di controllo di tre.

Aprire il `Basics.aspx` pagina, trascinare un ObjectDataSource dalla casella degli strumenti nella finestra di progettazione e fare clic sul collegamento Configura origine dati dal suo smart tag. Poiché il `ProductsBLL` è l'unica classe BLL che fornisce la modifica, inserimento ed eliminazione di metodi, configurare ObjectDataSource per utilizzare questa classe.


[![Configurare ObjectDataSource per utilizzare la classe ProductsBLL](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image10.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image9.png)

**Figura 5**: configurare ObjectDataSource per usare il `ProductsBLL` classe ([fare clic per visualizzare l'immagine ingrandita](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image11.png))


Nella schermata successiva è possibile specificare quali metodi del `ProductsBLL` classe viene eseguito il mapping di ObjectDataSource `Select()`, `Insert()`, `Update()`, e `Delete()` selezionando la scheda appropriata e scegliendo il metodo dall'elenco a discesa. Figura 6, che dovrebbe essere familiare a questo punto, viene eseguito il mapping di ObjectDataSource `Select()` metodo il `ProductsBLL` della classe `GetProducts()` metodo. Il `Insert()`, `Update()`, e `Delete()` metodi possono essere configurati selezionando la scheda appropriata dall'elenco nella parte superiore.


[![Avere ObjectDataSource restituire tutti i prodotti](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image13.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image12.png)

**Figura 6**: sono il ObjectDataSource restituire tutti i prodotti ([fare clic per visualizzare l'immagine ingrandita](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image14.png))


Nelle figure 7, 8 e 9 Mostra UPDATE, INSERT e DELETE di ObjectDataSource schede. Configurare le schede in modo che il `Insert()`, `Update()`, e `Delete()` metodi richiamano il `ProductsBLL` della classe `UpdateProduct`, `AddProduct`, e `DeleteProduct` metodi, rispettivamente.


[![Eseguire il mapping Update () metodo di ObjectDataSource al metodo UpdateProduct della classe ProductBLL](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image16.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image15.png)

**Figura 7**: eseguire il mapping di ObjectDataSource `Update()` metodo per il `ProductBLL` della classe `UpdateProduct` metodo ([fare clic per visualizzare l'immagine ingrandita](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image17.png))


[![Eseguire il mapping Insert () metodo di ObjectDataSource al metodo AddProduct della classe ProductBLL](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image19.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image18.png)

**Figura 8**: eseguire il mapping di ObjectDataSource `Insert()` metodo per il `ProductBLL` Add della classe `Product` metodo ([fare clic per visualizzare l'immagine ingrandita](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image20.png))


[![Eseguire il mapping Delete () metodo di ObjectDataSource al metodo DeleteProduct della classe ProductBLL](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image22.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image21.png)

**Figura 9**: eseguire il mapping di ObjectDataSource `Delete()` metodo per il `ProductBLL` della classe `DeleteProduct` metodo ([fare clic per visualizzare l'immagine ingrandita](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image23.png))


Si può notare che gli elenchi a discesa nelle schede UPDATE, INSERT e DELETE dispone già di questi metodi selezionati. Grazie all'uso dei dati si tratta di `DataObjectMethodAttribute` che decora i metodi di `ProducstBLL`. Ad esempio, il metodo DeleteProduct ha la firma seguente:


[!code-vb[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample2.vb)]

Il `DataObjectMethodAttribute` attributo indica lo scopo di ciascun metodo per la selezione, inserimento, aggiornamento, o l'eliminazione ed è o meno il valore predefinito. Se omesso di questi attributi durante la creazione di classi BLL, si sarà necessario selezionare manualmente i metodi da UPDATE, INSERT ed eliminare schede.

Dopo aver verificato che l'oggetto appropriato `ProductsBLL` metodi vengono eseguito il mapping di ObjectDataSource `Insert()`, `Update()`, e `Delete()` metodi, fare clic su Fine per completare la procedura guidata.

## <a name="examining-the-objectdatasources-markup"></a>Esaminando il Markup di ObjectDataSource

Dopo aver configurato ObjectDataSource mediante la procedura guidata, passare alla visualizzazione origine per esaminare il markup dichiarativo generato. Il `<asp:ObjectDataSource>` tag specifica l'oggetto sottostante e i metodi da richiamare. Esistono inoltre `DeleteParameters`, `UpdateParameters`, e `InsertParameters` che il mapping a parametri di input per il `ProductsBLL` della classe `AddProduct`, `UpdateProduct`, e `DeleteProduct` metodi:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample3.aspx)]

ObjectDataSource include un parametro per ognuno dei parametri di input per i metodi associati, come un elenco di `SelectParameter` s è presente quando ObjectDataSource è configurato per chiamare un metodo di selezione che prevede un parametro di input (ad esempio `GetProductsByCategoryID(categoryID)`). Come possiamo vedere più avanti, i valori di queste `DeleteParameters`, `UpdateParameters`, e `InsertParameters` vengono impostate automaticamente dal GridView, DetailsView e FormView prima di richiamare il ObjectDataSource `Insert()`, `Update()`, o `Delete()` metodo. Questi valori possono essere impostati anche a livello di codice in base alle esigenze, come verrà illustrato in un'esercitazione future.

Un effetto collaterale di utilizzando la procedura guidata per configurare in modo da ObjectDataSource è che Visual Studio imposta la [proprietà OldValuesParameterFormatString](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.oldvaluesparameterformatstring(VS.80).aspx) a `original_{0}`. Valore di questa proprietà viene utilizzato per includere i valori originali dei dati in fase di modifica ed è utile in due scenari:

- Se, quando si modifica un record, gli utenti sono in grado di modificare il valore di chiave primaria. In questo caso, sia il nuovo valore di chiave primaria e il valore di chiave primaria originale deve essere forniti in modo che il record con il valore di chiave primaria originale può essere trovato e il valore aggiornato di conseguenza.
- Quando si utilizza la concorrenza ottimistica. La concorrenza ottimistica è una tecnica per garantire che due utenti simultanei non sovrascrivere le modifiche apportate da altri e l'argomento per un'esercitazione in futura.

Il `OldValuesParameterFormatString` proprietà indica il nome dei parametri di input, l'aggiornamento dell'oggetto sottostante e i metodi di eliminazione per i valori originali. Questa proprietà e il relativo scopo in maggiore dettaglio parleremo quando si Esplora per la concorrenza ottimistica. È visualizzata a questo punto, tuttavia, poiché i metodi del nostro BLL non prevedono i valori originali e pertanto è importante che questa proprietà è la rimozione. Lasciando il `OldValuesParameterFormatString` proprietà è impostata su un valore diverso da quello predefinito (`{0}`) verrà generato un errore quando un controllo Web di dati tenta di richiamare il ObjectDataSource `Update()` o `Delete()` metodi perché verrà ObjectDataSource tentativo di passare in entrambe le `UpdateParameters` o `DeleteParameters` specificato oltre ai parametri di valore originale.

Se questo non è molto chiaro a questo punto, non preoccuparti, esamineremo questa proprietà e l'utilità in un'esercitazione future. Per il momento, solo assicurarsi di rimuovere la dichiarazione di proprietà interamente dalla sintassi dichiarativa o impostare il valore per il valore predefinito ({0}).

> [!NOTE]
> Se si deseleziona semplicemente di `OldValuesParameterFormatString` valore della proprietà dalla finestra delle proprietà nella visualizzazione progettazione, la proprietà continuano a esistere nella sintassi dichiarativa, ma essere impostata su una stringa vuota. Questa operazione, purtroppo, ancora comporterà lo stesso problema descritto sopra. Pertanto, rimuovere la proprietà completamente la sintassi dichiarativa o la finestra proprietà imposta il valore per l'impostazione predefinita, `{0}`.


## <a name="step-3-adding-a-data-web-control-and-configuring-it-for-data-modification"></a>Passaggio 3: Aggiunta di un controllo Web di dati e configurarlo per la modifica dei dati

Una volta ObjectDataSource è stato aggiunto alla pagina e configurato, si è pronti per aggiungere dati a controlli Web per la pagina per visualizzare i dati e forniscono un mezzo per l'utente finale per modificarlo. Verrà esaminato il GridView, DetailsView e FormView separatamente, come i controlli Web di dati diversificano in termini di funzionalità di modifica dei dati e configurazione.

Come si vedrà nella parte restante di questo articolo, aggiunta di base di modifica, inserimento ed eliminazione di supporto tramite GridView, DetailsView, e FormView è effettivamente è così semplice come controllare un paio di caselle di controllo. Esistono molti sfumature e casi limite in tutto il mondo reale che rendono tale funzionalità più complessa rispetto a punto è sufficiente e fare clic su. Tuttavia, questa esercitazione è incentrata esclusivamente su dimostra la funzionalità di modifica dei dati semplice. Nelle esercitazioni successive verranno esaminati i problemi che sorgono senza dubbio in un'impostazione del mondo reale.

## <a name="deleting-data-from-the-gridview"></a>L'eliminazione dei dati dal controllo GridView.

Per iniziare, trascinare un controllo GridView dalla casella degli strumenti nella finestra di progettazione. Successivamente, è possibile associare ObjectDataSource a GridView selezionandolo dall'elenco a discesa smart tag del controllo GridView. A questo punto sarà markup dichiarativo del controllo GridView:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample4.aspx)]

Associazione di GridView a ObjectDataSource mediante smart tag offre due vantaggi:

- BoundField e CheckBoxFields vengono create automaticamente per ognuno dei campi restituiti da ObjectDataSource. Inoltre, il BoundField e del CheckBoxField sono impostate in base ai metadati del campo sottostante. Ad esempio, il `ProductID`, `CategoryName`, e `SupplierName` campi sono contrassegnati come di sola lettura nel `ProductsDataTable` e pertanto non deve essere aggiornabile quando si modifica. Per supportare questo, questi BoundField [proprietà ReadOnly](https://msdn.microsoft.com/library/system.web.ui.webcontrols.boundfield.readonly(VS.80).aspx) sono impostate su `True`.
- Il [proprietà DataKeyNames](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.datakeynames(VS.80).aspx) viene assegnato a campi di chiave primaria dell'oggetto sottostante. Questa operazione è essenziale quando utilizzando GridView per la modifica o eliminazione di dati, come questa proprietà indica il campo (o set di campi) univoco che identifica ogni record. Per ulteriori informazioni sul `DataKeyNames` proprietà, fare riferimento al [Master/dettaglio con un controllo DetailView per i dettagli di Master GridView selezionabile](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) esercitazione.

Mentre il controllo GridView può essere associato a ObjectDataSource mediante la finestra proprietà o la sintassi dichiarativa, in questo modo è necessario aggiungere manualmente il BoundField appropriato e `DataKeyNames` markup.

Il controllo GridView fornisce supporto incorporato per la modifica a livello di riga e l'eliminazione. Configurazione di un controllo GridView per supportare l'eliminazione aggiunge una colonna di pulsanti di eliminazione. Quando l'utente finale fa clic sul pulsante Elimina per una determinata riga, viene utilizzata un postback e GridView esegue i passaggi seguenti:

1. ObjectDataSource `DeleteParameters` assegnati uno o più valori
2. ObjectDataSource `Delete()` metodo viene richiamato, l'eliminazione di record specificato
3. GridView riassocia a ObjectDataSource richiamando il `Select()` (metodo)

I valori assegnati al `DeleteParameters` sono i valori del `DataKeyNames` uno o più campi per la riga è stato fatto clic con pulsante Elimina. Di conseguenza è essenziale che un GridView `DataKeyNames` correttamente impostata. Se è presente, il `DeleteParameters` verrà assegnato un valore di `Nothing` nel passaggio 1, che a sua volta non comporterà qualsiasi record eliminato nel passaggio 2.

> [!NOTE]
> Il `DataKeys` insieme è memorizzato lo stato del controllo GridView s, il che significa che il `DataKeys` valori verranno mantenuti durante il postback anche se lo stato di visualizzazione GridView s è stato disabilitato. Tuttavia, è molto importante che lo stato di visualizzazione rimane abilitato per GridView che supportano la modifica o eliminazione (comportamento predefinito). Se si imposta la s GridView `EnableViewState` proprietà `false`, la modifica ed eliminazione di comportamento funziona correttamente per un singolo utente, ma se sono presenti utenti simultanei, l'eliminazione dei dati, esiste la possibilità che tali utenti simultanei potrebbero accidentalmente eliminazione o modifica di record che non prevede. Vedere il post di blog, [avviso: problema di concorrenza con ASP.NET 2.0 GridView/DetailsView/FormViews che supportano la modifica e/o l'eliminazione e il cui stato di visualizzazione è disabilitato](http://scottonwriting.net/sowblog/posts/10054.aspx), per ulteriori informazioni.


Questo avviso stesso vale anche per DetailsViews e FormViews.

Per aggiungere funzionalità di eliminazione a un controllo GridView, passare allo smart tag e selezionare la casella di controllo Abilita eliminazione.


![Verificare l'abilitazione di eliminazione di casella di controllo](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image24.png)

**Figura 10**: verificare l'abilitazione di eliminazione di casella di controllo


La casella di controllo Abilita eliminazione dallo smart tag aggiunge un CommandField a GridView. Il CommandField esegue il rendering di una colonna in GridView con pulsanti per l'esecuzione di uno o più delle seguenti attività: la selezione di un record, la modifica di un record e l'eliminazione di un record. Si è osservato in precedenza CommandField nell'azione con la selezione dei record nel [Master/dettaglio con un controllo DetailView per i dettagli di Master GridView selezionabile](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) esercitazione.

Il CommandField contiene un numero di `ShowXButton` le proprietà che indicano le serie di pulsanti vengono visualizzati nel CommandField. Selezionando la casella di controllo un CommandField Abilita eliminazione cui `ShowDeleteButton` proprietà `True` è stato aggiunto alla raccolta di colonne del controllo GridView.


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample5.aspx)]

A questo punto, può sembrare strano, è stata completata con l'aggiunta del supporto di eliminazione a GridView. Come mostrato nella figura 11, quando visitare questa pagina tramite un browser di una colonna di pulsanti di eliminazione è presente.


[![Il CommandField aggiunge una colonna di pulsanti di Delete](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image26.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image25.png)

**Figura 11**: il CommandField aggiunge una colonna di eliminare i pulsanti ([fare clic per visualizzare l'immagine ingrandita](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image27.png))


Se stai creando questa esercitazione interamente per conto proprio, durante il test di questa pagina facendo clic sul pulsante Elimina genererà un'eccezione. Continuare la lettura di informazioni perché in cui sono state generate queste eccezioni e come risolverli.

> [!NOTE]
> Se procedendo con download disponibile in questa esercitazione, questi problemi sono già stati considerati per. Consiglia, tuttavia, per leggere i dettagli riportati di seguito per identificare i problemi che possono verificarsi e soluzioni alternative adatti.


Se, durante il tentativo di eliminare un prodotto, verrà generata un'eccezione il cui messaggio è simile a "*ObjectDataSource 'ObjectDataSource1' non è riuscito a trovare un metodo non generico 'DeleteProduct' che dispone di parametri: productID, originale\_ ProductID*, "si è probabilmente dimenticato di rimuovere il `OldValuesParameterFormatString` proprietà da ObjectDataSource. Con il `OldValuesParameterFormatString` proprietà specificata, ObjectDataSource tenta di passare in entrambi `productID` e `original_ProductID` parametri di input con il `DeleteProduct` metodo. `DeleteProduct`, tuttavia, accetta solo un singolo parametro di input, pertanto l'eccezione. Rimozione di `OldValuesParameterFormatString` proprietà (o se è impostato su `{0}`) indica ObjectDataSource non tentare di passare il parametro di input originale.


[![Verificare che la proprietà OldValuesParameterFormatString è stata cancellata](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image29.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image28.png)

**Figura 12**: verificare che il `OldValuesParameterFormatString` proprietà è stata cancellata Out ([fare clic per visualizzare l'immagine ingrandita](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image30.png))


Anche se è stato rimosso il `OldValuesParameterFormatString` proprietà, è comunque verrà generata un'eccezione durante il tentativo di eliminare un prodotto con il messaggio: "*l'istruzione DELETE in conflitto con il vincolo di riferimento ' FK\_ordine\_dettagli \_Prodotti*. " Il database Northwind contiene un vincolo di chiave esterna tra la `Order Details` e `Products` vale a dire che un prodotto non può essere eliminato dal sistema se sono presenti uno o più record nella tabella di `Order Details` tabella. Poiché ogni prodotto nel database Northwind dispone di almeno un record in `Order Details`, fino a quando non è prima di eliminare i record di dettagli ordine associato del prodotto non è possibile eliminare tutti i prodotti.


[![Un vincolo Foreign Key impedisce l'eliminazione dei prodotti](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image32.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image31.png)

**Figura 13**: un vincolo Foreign Key impedisce l'eliminazione di prodotti ([fare clic per visualizzare l'immagine ingrandita](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image33.png))


Per questa esercitazione, si sarà sufficiente eliminare tutti i record di `Order Details` tabella. In un'applicazione reale è necessario uno:

- Dispone di un'altra schermata per gestire le informazioni sui dettagli dell'ordine
- Aumentare la `DeleteProduct` per includere la logica per eliminare i dettagli dell'ordine del prodotto specificato (metodo)
- Modificare la query SQL utilizzata dal TableAdapter per includere l'eliminazione di dettagli dell'ordine del prodotto specificato

Solo eliminare tutti i record di `Order Details` per aggirare il vincolo di chiave esterno nella tabella. Accedere a Esplora Server in Visual Studio, fare clic su di `NORTHWND.MDF` nodo e scegliere Nuova Query. Quindi, nella finestra query, eseguire l'istruzione SQL seguente: `DELETE FROM [Order Details]`


[![Eliminare tutti i record dalla tabella Dettagli ordine](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image35.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image34.png)

**Figura 14**: Elimina tutti i record il `Order Details` tabella ([fare clic per visualizzare l'immagine ingrandita](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image36.png))


Dopo aver cancellato di `Order Details` tabella facendo clic sul pulsante Elimina eliminerà il prodotto senza errori. Se facendo clic sul pulsante Elimina non elimina il prodotto, verificare che il controllo GridView `DataKeyNames` proprietà è impostata per il campo chiave primaria (`ProductID`).

> [!NOTE]
> Quando si fa clic sul pulsante Elimina viene utilizzata un postback e viene eliminato il record. Può essere pericoloso poiché è facile accidentalmente fare clic sul pulsante di eliminazione della riga non corretto. In un'esercitazione future vedremo come aggiungere un messaggio di conferma sul lato client quando si elimina un record.


## <a name="editing-data-with-the-gridview"></a>La modifica dei dati con il controllo GridView.

Con l'eliminazione, il controllo GridView fornisce inoltre supporto integrato di modificando a livello di riga. Configurazione di un controllo GridView per supportare la modifica aggiunge una colonna di pulsanti di modifica. Dal punto di vista dell'utente finale, fare clic su Modifica pulsante le cause una riga che riga diventerà modificabile, attivare le celle nelle caselle di testo contenente i valori esistenti e sostituire il pulsante di modifica con l'aggiornamento e i pulsanti Annulla. Dopo aver apportato le modifiche desiderate, l'utente finale possibile fare clic sul pulsante di aggiornamento per il commit delle modifiche o Annulla per ignorarle. In entrambi i casi, dopo aver selezionato l'aggiornamento o Annulla GridView restituisce lo stato di pre-modifica.

La prospettiva come lo sviluppatore della pagina, quando l'utente finale fa clic sul pulsante Modifica per una determinata riga, viene utilizzata un postback e GridView esegue i passaggi seguenti:

1. Il GridView `EditItemIndex` proprietà viene assegnato per l'indice della riga è stato fatto clic sul pulsante la cui modifica
2. GridView riassocia a ObjectDataSource richiamando il `Select()` (metodo)
3. L'indice di riga che corrisponde il `EditItemIndex` viene eseguito il rendering in "modalità di modifica". In questa modalità, il pulsante Modifica è sostituito da pulsanti Aggiorna e Annulla e BoundField cui `ReadOnly` proprietà sono False (impostazione predefinita) vengono sottoposti a rendering come i controlli Web di casella di testo la cui `Text` proprietà assegnate ai valori dei campi dati.

A questo punto il markup viene restituito al browser, consentendo all'utente finale di apportare eventuali modifiche ai dati della riga. Quando l'utente fa clic sul pulsante di aggiornamento, si verifica un postback e GridView esegue i passaggi seguenti:

1. ObjectDataSource `UpdateParameters` valori vengono assegnati i valori immessi dall'utente finale nell'interfaccia di modifica del controllo GridView
2. ObjectDataSource `Update()` metodo viene richiamato, l'aggiornamento di record specificato
3. GridView riassocia a ObjectDataSource richiamando il `Select()` (metodo)

I valori di chiave primaria assegnati per il `UpdateParameters` nel passaggio 1 provengono dai valori specificati nel `DataKeyNames` proprietà, mentre i valori di chiave non primaria provengono dal testo nei controlli casella di testo Web per la riga modificata. Come con l'eliminazione, è essenziale che un GridView `DataKeyNames` correttamente impostata. Se è presente, il `UpdateParameters` valore di chiave primaria verrà assegnato un valore di `Nothing` nel passaggio 1, che a sua volta non comporterà tutti i record aggiornati nel passaggio 2.

Controllando semplicemente la casella di controllo Abilita modifica smart tag del controllo GridView, è possibile attivare la funzionalità di modifica.


![Verificare l'abilitazione, la casella di controllo di modifica](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image37.png)

**Figura 15**: verificare l'abilitazione di casella di controllo di modifica


Controllare la casella di controllo Abilita modifica aggiungerà un CommandField (se necessario) e impostare il relativo `ShowEditButton` proprietà `True`. Come abbiamo visto prima il CommandField contiene un numero di `ShowXButton` le proprietà che indicano le serie di pulsanti vengono visualizzati nel CommandField. La casella di controllo Abilita modifica aggiunge il `ShowEditButton` proprietà per il CommandField esistente:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample6.aspx)]

Questo è tutto per l'aggiunta del supporto di modificando rudimentale. Come illustrato di seguito Figure16, l'interfaccia di modifica è piuttosto essenziale è costituita dall'ogni BoundField cui `ReadOnly` è impostata su `False` (predefinito) viene visualizzato come una casella di testo. Sono inclusi i campi come `CategoryID` e `SupplierID`, che sono chiavi ad altre tabelle.


[![Facendo clic sul pulsante Modifica s Chai viene visualizzata la riga in modalità di modifica](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image39.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image38.png)

**Figura 16**: s Chai facendo clic sul pulsante Modifica consente di visualizzare la riga in modalità di modifica ([fare clic per visualizzare l'immagine ingrandita](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image40.png))


Oltre a chiedere agli utenti di modificare direttamente i valori di chiave esterna, l'interfaccia dell'interfaccia modifica risulta mancante nei modi seguenti:

- Se l'utente immette un `CategoryID` o `SupplierID` non esiste nel database, il `UPDATE` violeranno un vincolo di chiave esterna, generare un'eccezione da generare.
- L'interfaccia di modifica non include alcuna convalida. Se non si fornisce un valore obbligatorio (ad esempio `ProductName`), oppure immettere un valore stringa in cui è previsto un valore numerico (ad esempio, immettere "Troppo elevato!" nel `UnitPrice` casella di testo), verrà generata un'eccezione. Un'esercitazione in futura verrà esaminato aggiungere i controlli di convalida per l'interfaccia utente di modifica.
- Attualmente, *tutti* campi prodotto che non sono di sola lettura devono essere incluso nel controllo GridView. Se si dovesse rimuovere un campo in GridView, ad esempio `UnitPrice`, quando l'aggiornamento dei dati di GridView non imposta il `UnitPrice` `UpdateParameters` valore, che comporterebbe la modifica del record del database `UnitPrice` per un `NULL` valore. Analogamente, se un campo obbligatorio, ad esempio `ProductName`, viene rimosso dal controllo GridView, l'aggiornamento avrà esito negativo con lo stesso "*colonna 'ProductName' non ammette valori null*" eccezione indicato in precedenza.
- La modifica dell'interfaccia formattazione migliorate per essere desiderato. Il `UnitPrice` viene visualizzato con quattro cifre decimali. Idealmente il `CategoryID` e `SupplierID` valori conterrebbe controlli DropDownList che elenca le categorie e i fornitori nel sistema.

Questi sono tutti i punti deboli che abbiamo a live con per il momento, ma verranno risolti nelle esercitazioni successive.

## <a name="inserting-editing-and-deleting-data-with-the-detailsview"></a>Inserimento, modifica ed eliminazione di dati con il controllo DetailsView.

Come abbiamo visto nelle esercitazioni precedenti, il controllo DetailsView visualizza un record alla volta, ad esempio GridView, consente la modifica ed eliminazione del record attualmente visualizzato. Esperienza di entrambi dell'utente finale con la modifica e l'eliminazione di elementi da un controllo DetailsView e il flusso di lavoro dal lato ASP.NET è identico a quello di GridView. In DetailsView è diverso dal controllo GridView è fornisce inoltre supporto integrato di inserimento.

Per illustrare le funzionalità di modifica di dati di GridView, per iniziare, aggiungere un controllo DetailsView per il `Basics.aspx` pagina sopra il GridView esistente e associarlo a ObjectDataSource esistente tramite smart tag del controllo DetailsView. Successivamente, cancellare il contenuto del controllo DetailsView `Height` e `Width` , proprietà e selezionare l'opzione Abilita Paging smart tag. Per consentire la modifica, inserimento ed eliminazione di supporto, semplicemente selezionare le caselle di controllo Abilita modifica, Abilita inserimento ed eliminazione di abilitare nello smart tag.


![Configurare il controllo DetailsView per supportare la modifica, inserimento ed eliminazione](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image41.png)

**Figura 17**: configurare il controllo DetailsView per supportare la modifica, inserimento ed eliminazione


Come GridView, aggiunta di modifica, inserimento o eliminazione di supporto viene aggiunto un CommandField al controllo DetailsView., come illustrato nella sintassi dichiarativa seguente:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample7.aspx)]

Si noti che per il CommandField DetailsView viene visualizzato alla fine della raccolta di colonne per impostazione predefinita. Dal momento i del controllo DetailsView campi vengono visualizzati come righe, il CommandField viene visualizzato come una riga con l'istruzione Insert, modificare ed eliminare i pulsanti nella parte inferiore del controllo DetailsView.


[![Configurare il controllo DetailsView per supportare la modifica, inserimento ed eliminazione](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image43.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image42.png)

**Figura 18**: configurare DetailsView per supportano la modifica, inserimento ed eliminazione ([fare clic per visualizzare l'immagine ingrandita](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image44.png))


Facendo clic sul pulsante Elimina viene avviata la stessa sequenza di eventi come con GridView: un postback. seguito dal controllo DetailsView popolamento ObjectDataSource `DeleteParameters` in base il `DataKeyNames` i valori e completata con una chiamata ObjectDataSource `Delete()` metodo, che è effettivamente il prodotto viene rimosso dal database. La modifica nel controllo DetailsView funziona anche in modo identico a quello di GridView.

Per l'inserimento, l'utente finale è presentato con un nuovo pulsante che, quando si fa clic, esegue il rendering di DetailsView in "modalità di inserimento." Con "modalità di inserimento" pulsante nuovo viene sostituito da pulsanti Inserisci e Annulla e solo tali BoundField cui `InsertVisible` è impostata su `True` (predefinito) vengono visualizzati. I campi dati identificati come campi di incremento automatico, ad esempio `ProductID`, hanno loro [InsertVisible proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datacontrolfield.insertvisible(VS.80).aspx) impostato su `False` durante l'associazione di DetailsView all'origine dati tramite lo smart tag.

Quando si associa un'origine dati a un controllo DetailsView tramite lo smart tag, Visual Studio imposta la `InsertVisible` proprietà `False` solo per i campi di incremento automatico. I campi di sola lettura, ad esempio `CategoryName` e `SupplierName`, verrà visualizzato nell'interfaccia utente "modalità di inserimento", a meno che loro `InsertVisible` è impostata in modo esplicito su `False`. È opportuno impostare questi due campi `InsertVisible` proprietà `False`, tramite la sintassi dichiarativa di DetailsView o modifica campi collegare lo smart tag. Figura 19 viene illustrata l'impostazione di `InsertVisible` proprietà `False` facendo clic su Modifica campi di collegamento.


[![Northwind Traders offre ora tè Acme](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image46.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image45.png)

**Figura 19**: Northwind Traders ora offre Acme tè ([fare clic per visualizzare l'immagine ingrandita](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image47.png))


Dopo aver impostato il `InsertVisible` proprietà, visualizzazione di `Basics.aspx` pagina in un browser e fare clic sul pulsante nuovo. Figura 20 mostra DetailsView quando si aggiunge una nuovo bevande, tè Acme, per la linea di prodotti.


[![Northwind Traders offre ora tè Acme](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image49.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image48.png)

**Figura 20**: Northwind Traders ora offre Acme tè ([fare clic per visualizzare l'immagine ingrandita](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image50.png))


Dopo aver immettere i dettagli per Acme tè e fare clic sul pulsante di inserimento, viene utilizzata un postback e viene aggiunto il nuovo record per il `Products` tabella di database. Poiché questo DetailsView sono elencati i prodotti nell'ordine con cui presenti nella tabella di database, è necessario pagina e l'ultimo prodotto per vedere il nuovo prodotto.


[![Dettagli per tè Acme](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image52.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image51.png)

**Figura 21**: i dettagli per Acme tè ([fare clic per visualizzare l'immagine ingrandita](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image53.png))


> [!NOTE]
> Di DetailsView [CurrentMode proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.currentmode(VS.80).aspx) indica l'interfaccia viene visualizzata e può essere uno dei seguenti valori: `Edit`, `Insert`, o `ReadOnly`. Il [proprietà DefaultMode](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.defaultmode(VS.80).aspx) indica la modalità DetailsView restituisce dopo una modifica o inserimento è stata completata ed è utile per la visualizzazione di un controllo DetailsView in modo permanente in modalità di modifica o modalità di inserimento.


Il punto e fare clic su inserimento e modifica di funzionalità del controllo DetailsView subisce le stesse limitazioni di GridView: l'utente deve immettere esistente `CategoryID` e `SupplierID` valori tramite una casella di testo; manca l'interfaccia una logica di convalida; tutti i campi di prodotto che non consentono `NULL` valori o non ha un valore predefinito valore specificato a livello di database deve essere incluso nell'inserimento interfaccia e così via.

Le tecniche verrà esaminato per l'estensione e sul miglioramento interfaccia di modifica del controllo GridView in futuro articoli possono essere applicati a modifica e l'inserimento di nonché le interfacce del controllo DetailsView.

## <a name="using-the-formview-for-a-more-flexible-data-modification-user-interface"></a>Utilizzo di FormView per un'interfaccia utente di modifica dei dati più flessibile

FormView offre supporto incorporato per l'inserimento, modifica ed eliminazione di dati, ma poiché utilizza i modelli anziché i campi non è di aggiungere il BoundField o il CommandField usato dai controlli GridView e DetailsView per fornire i dati interfaccia di modifica. Al contrario, questa interfaccia, i controlli Web per la raccolta utente quando si aggiunge un nuovo elemento di input o modifica uno esistente con nuovo, modificare, eliminare, inserimento, aggiornamento e annullare i pulsanti deve essere aggiunti manualmente per i modelli appropriati. Fortunatamente, Visual Studio crea automaticamente l'interfaccia necessaria quando si associa il controllo FormView a un'origine dati tramite l'elenco a discesa in smart tag.

Per illustrare queste tecniche, per iniziare, aggiungere un controllo FormView per il `Basics.aspx` pagina e, dallo smart tag del controllo FormView, associarlo a ObjectDataSource già creato. Verrà generato un `EditItemTemplate`, `InsertItemTemplate`, e `ItemTemplate` per FormView con controlli TextBox Web per la raccolta di input dell'utente e i controlli pulsante Web per il nuovo, modificare, eliminare, Insert, Update e pulsanti Annulla. Inoltre, il FormView `DataKeyNames` proprietà è impostata per il campo chiave primaria (`ProductID`) dell'oggetto restituito da ObjectDataSource. Infine, selezionare l'opzione Abilita Paging nello smart tag del controllo FormView.

Di seguito è illustrato il markup dichiarativo per il controllo FormView `ItemTemplate` dopo aver associato il controllo FormView a ObjectDataSource. Per impostazione predefinita, ogni campo product valore booleano non è associato al `Text` proprietà di un controllo etichetta Web mentre ogni campo di valore booleano (`Discontinued`) è associato il `Checked` proprietà di un controllo casella di controllo Web disabilitato. Affinché i pulsanti New, Edit e Delete attivare determinati comportamenti del controllo FormView quando si fa clic, è fondamentale che i relativi `CommandName` valori impostati su `New`, `Edit`, e `Delete`, rispettivamente.


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample8.aspx)]

La figura 22 illustra il controllo FormView `ItemTemplate` quando viene visualizzato tramite un browser. Ogni campo di prodotto viene elencato con i pulsanti nella parte inferiore di New, Edit e Delete.


[![ItemTemplate disporranno delle FormView Elenca ogni campo Product insieme ai nuovi, modificare ed eliminare i pulsanti](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image55.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image54.png)

**Figura 22**: il controllo FormView disporranno delle `ItemTemplate` Elenca ogni prodotto campo lungo con New, Edit e Delete pulsanti ([fare clic per visualizzare l'immagine ingrandita](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image56.png))


Come con GridView e DetailsView, fare clic sul pulsante Elimina o qualsiasi pulsante, LinkButton o ImageButton cui `CommandName` proprietà è impostata su Delete causa un postback, popola di ObjectDataSource `DeleteParameters` basato sul controllo FormView `DataKeyNames`valore e richiama il ObjectDataSource `Delete()` metodo.

Quando si fa clic sul pulsante Modifica viene utilizzata un postback e i dati vengano riassociati al `EditItemTemplate`, che è responsabile per l'interfaccia di modifica di rendering. Questa interfaccia include i controlli Web per la modifica di dati con i pulsanti Annulla e di aggiornamento. Il valore predefinito `EditItemTemplate` generato da Visual Studio contiene un'etichetta per tutti i campi a incremento automatico (`ProductID`), una casella di testo per ogni campo di valori non booleani e una casella di controllo per ogni campo di valore booleano. Questo comportamento è molto simile al BoundField generato automaticamente nei controlli GridView e DetailsView.

> [!NOTE]
> Un problema di piccole dimensioni con generazione automatica del controllo FormView del `EditItemTemplate` è che viene eseguito il rendering TextBox Web controlli per i campi che sono di sola lettura, ad esempio `CategoryName` e `SupplierName`. Vedremo come account per l'oggetto a breve.


I controlli casella di testo il `EditItemTemplate` hanno loro `Text` proprietà associato al valore del campo dati corrispondente using *l'associazione dati bidirezionale*. Associazione dati bidirezionale, identificato da `<%# Bind("dataField") %>`, esegue l'associazione dati sia durante l'associazione dati al modello e durante il popolamento di parametri di ObjectDataSource per l'inserimento o modifica di record. Ovvero, quando l'utente fa clic sul pulsante di modifica dal `ItemTemplate`, il `Bind()` metodo restituisce il valore del campo dati specificato. Dopo che l'utente apporta modifiche e fa clic sull'aggiornamento, i valori registrati back che corrispondono ai campi dati specificati mediante `Bind()` vengono applicate a ObjectDataSource `UpdateParameters`. In alternativa, l'associazione dati unidirezionale, identificato da `<%# Eval("dataField") %>`, solo recupera i valori di campo dati quando si associano dati al modello e *non* restituiscono i valori immessi dall'utente per i parametri dell'origine dati durante il postback.

Il markup dichiarativo seguente viene illustrato il controllo FormView `EditItemTemplate`. Si noti che il `Bind()` metodo viene utilizzato in seguito la sintassi di associazione dati e i controlli Web Update e annullare pulsante con i relativi `CommandName` proprietà impostare di conseguenza.


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample9.aspx)]

Il nostro `EditItemTemplate`, a questo punto, verrà generata un'eccezione viene generata se si tenta di utilizzarla. Il problema è che il `CategoryName` e `SupplierName` i campi vengono visualizzati come controlli TextBox Web il `EditItemTemplate`. È necessario modificare queste caselle di testo a etichette o rimuoverli completamente. Verrà semplicemente eliminarli completamente il `EditItemTemplate`.

Nella figura 23 Mostra FormView in un browser dopo che il pulsante Modifica è stato fatto clic Chai. Si noti che il `SupplierName` e `CategoryName` campi visualizzati di `ItemTemplate` non sono più presenti, come è stato rimosso dal `EditItemTemplate`. Quando si fa clic sul pulsante Aggiorna FormView procede attraverso la stessa sequenza di passaggi come i controlli GridView e DetailsView.


[![Per impostazione predefinita il EditItemTemplate Mostra ogni campo modificabile prodotto come una casella di testo o una casella di controllo](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image58.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image57.png)

**Nella figura 23**: per impostazione predefinita, il `EditItemTemplate` Mostra ogni prodotto campo modificabile come una casella di testo o una casella di controllo ([fare clic per visualizzare l'immagine ingrandita](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image59.png))


Quando il pulsante Inserisci viene selezionato il controllo FormView `ItemTemplate` viene utilizzata un postback. Tuttavia, i dati non sono associati a FormView perché viene aggiunto un nuovo record. Il `InsertItemTemplate` interfaccia include i controlli Web per l'aggiunta di un nuovo record con i pulsanti Inserisci e Annulla. Il valore predefinito `InsertItemTemplate` generato da Visual Studio contiene una casella di testo per ogni campo di valori non booleani e una casella di controllo per ogni campo di valore Boolean, simile a generazione automatica `EditItemTemplate`dell'interfaccia. I controlli casella di testo hanno loro `Text` proprietà associato al valore di loro corrispondente campo di dati mediante l'associazione dati bidirezionale.

Il markup dichiarativo seguente viene illustrato il controllo FormView `InsertItemTemplate`. Si noti che il `Bind()` viene utilizzato il metodo in seguito la sintassi di associazione dati e i controlli Web di pulsante Annulla e di inserimento con loro `CommandName` proprietà impostare di conseguenza.


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample10.aspx)]

È abbastanza particolare con generazione automatica del controllo FormView del `InsertItemTemplate`. In particolare, i controlli casella di testo Web vengono creati anche per i campi che sono di sola lettura, ad esempio `CategoryName` e `SupplierName`. Come con la `EditItemTemplate`, è necessario rimuovere queste caselle di testo dal `InsertItemTemplate`.

Figura 24 Mostra FormView in un browser quando si aggiunge un nuovo prodotto, Acme caffè. Si noti che il `SupplierName` e `CategoryName` campi visualizzati di `ItemTemplate` non sono più presenti, come è stato rimosso. Quando viene eseguito il controllo FormView tramite la stessa sequenza di passaggi del controllo DetailsView viene scelto il pulsante di inserimento, aggiunta di un nuovo record per il `Products` tabella. Figura 25 Mostra i dettagli del prodotto di caffè Acme in FormView dopo che è stato inserito.


[![Il InsertItemTemplate determina l'interfaccia di inserimento del controllo FormView](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image61.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image60.png)

**Figura 24**: il `InsertItemTemplate` determina l'interfaccia di inserimento del controllo FormView ([fare clic per visualizzare l'immagine ingrandita](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image62.png))


[![I dettagli per il nuovo prodotto, Coffee Acme, vengono visualizzati in FormView](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image64.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image63.png)

**Figura 25**: il dettagli per il nuovo prodotto, Coffee Acme, vengono visualizzati in FormView ([fare clic per visualizzare l'immagine ingrandita](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image65.png))


Separando out di sola lettura, la modifica e l'inserimento di interfacce in tre modelli distinti, FormView consente un maggiore grado di controllo su queste interfacce DetailsView e di GridView.

> [!NOTE]
> Ad esempio di DetailsView, FormView `CurrentMode` proprietà indica l'interfaccia viene visualizzata e il relativo `DefaultMode` proprietà indica la modalità FormView restituisce per dopo una modifica o inserimento è stata completata.


## <a name="summary"></a>Riepilogo

In questa esercitazione sono state esaminate le nozioni di base di inserimento, modifica ed eliminazione di dati mediante il controllo GridView, DetailsView e FormView. Tutti e tre i controlli forniscono un certo livello di funzionalità di modifica dei dati incorporati che possono essere utilizzate senza scrivere una singola riga di codice in una pagina ASP.NET grazie ai controlli Web di data e di ObjectDataSource. Tuttavia, il semplice e quindi le tecniche di rendering un piuttosto frail e interfaccia utente di modifica dati naïve. Per fornire la convalida, inserire valori a livello di codice, gestire le eccezioni, personalizzare l'interfaccia utente e così via, è necessario affidarsi a un insieme di tecniche che verranno analizzati tramite le esercitazioni più avanti.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Precedente](limiting-data-modification-functionality-based-on-the-user-cs.md)
> [Successivo](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)
